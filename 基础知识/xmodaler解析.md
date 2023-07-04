# xmodaler解析

## Data Pipeline（以coco为例）

流程：

```python
self.build_train_loader(cfg)
build_xmodaler_train_loader(cfg)
@configurable(from_config=_train_loader_from_config)
dataset_mapper = build_dataset_mapper(cfg, name=cfg.DATASETS.TRAIN, stage="train")
datalist = dataset_mapper.load_data(cfg)
dataset = DatasetFromList(datalist, copy=False)
dataset = MapDataset(dataset, dataset_mapper)
RandomSampler(dataset)
data_loader
```

### 预备知识

```python
@configurable
@configurable(from_config=_train_loader_from_config)
```

装饰器的作用：将被包装函数就行预处理，目的是解析cfg的参数。

1. 检查入参是否含有cfg

2. 如含有cfg，调用from_config方法，将cfg转换成参数字典，调用a_func。

3. 如无cfg，直接按原参数列表调用a_func

### build_dataset_mapper

这里就是构建数据处理类，即MSCoCoDataset。

初始化：由于MSCoCoDataset被configurable装饰，先通过from_config方法解析cfg再调用MSCoCoDataset的初始化。

datalist：采用MSCoCoDataset中load_data方法构建datalist。加载原始数据为datalist，若有额外的数据信息在这里处理添加进datalist中。

### dataset

由于我们的dataloder使用的是pytorch默认的，所以要将dataset的格式处理成其所接受的格式

DatasetFromList就是处理datalist为标准形式

MapDataset中使用了mapper。在整个数据集中一条数据对应的是一个dict，那么整个数据集就是list[dict]。之后我们再看`DatsetFromList`，它的`__getitem__`函数非常简单，它只是简单粗暴地就返回了指定idx的元素。显然这样是不行的，因为在把数据扔给模型训练之前我们肯定还要对数据做一定的处理，而这个工作就是由`mapper`来做的。

mapper在这里就是build_dataset_mapper中构建的MSCoCoDataset。在使用时调用call函数对数据进行处理，包括读取数据，一些增强操作都在这里完成。

在训练时的调用顺序：MapDataset中的`__getitem__`（其实就是主要调用MSCoCoDataset处理数据函数），DatsetFromList中的`__getitem__`

## Model Pipeline（以Transformer为例）

### 模型构建

在编码解码框架下，大多使用TransformerEncoderDecoder(BaseEncoderDecoder)类，模型的构建大多在BaseEncoderDecoder完成。

```python
return {
    "token_embed": build_embeddings(cfg, cfg.MODEL.TOKEN_EMBED.NAME),
    "visual_embed": build_embeddings(cfg, cfg.MODEL.VISUAL_EMBED.NAME),
    "encoder": build_encoder(cfg),
    "decoder": build_decoder(cfg),
    "predictor": build_predictor(cfg),
    "greedy_decoder": build_greedy_decoder(cfg),
    "beam_searcher": build_beam_searcher(cfg),
    "vocab_size": cfg.MODEL.VOCAB_SIZE,
    "max_seq_len": cfg.MODEL.MAX_SEQ_LEN
}
```

### 训练流程

```python
inputs = batched_inputs
masks = self.get_extended_attention_mask(batched_inputs)#掩码
ve_out = self.visual_embed(batched_inputs)#视觉位置编码
encoder_out_v = self.encoder(inputs, mode='v')
te_out = self.token_embed(batched_inputs)
encoder_out_t = self.encoder(inputs, mode='t')
inputs.update(encoder_out_t)
decoder_out = self.decoder(inputs)
tlogits = self.predictor(inputs)
vlogits = self.v_predictor(inputs)
```

## train-val-test Pipeline

### train

```python
self.before_train()
for self.iter in range(start_iter, max_iter):
    self.before_step()
    self.run_step()
    self.after_step()
self.iter += 1
self.after_train()
```

以上是train的整体流程，而这些函数所对应的内容有hook决定。

注册hook：

```python
hooks.IterationTimer()#时间监控
hooks.LRScheduler()#学习率调整
hooks.ScheduledSampling #计划采样，解决exposure bias
hooks.ModelWeightsManipulating()#before_train加载或初始化模型权重
#以下hook根据选择进行注册
hooks.PeriodicCheckpointer#保存模型指定内容
hooks.EvalHook#评估函数
```

在执行时按照注册顺序执行所对应hook中的before_train，before_step等方法。

run_step包含了主要的训练逻辑

```python
data = next(self._train_data_loader_iter)
data = comm.unwrap_model(self.model).preprocess_batch(data)
data[kfg.SS_PROB] = self.ss_prob
outputs_dict = self.model(data)
losses_dict = {}
for loss in self.losses:
    loss_dict = loss(outputs_dict)
    losses_dict.update(loss_dict)
losses = [losses_dict[k] for k in losses_dict if 'acc' not in k]
losses = sum(losses)
self.optimizer.zero_grad()
losses.backward()
self._write_metrics(losses_dict, data_time)
self.optimizer.step()
```

