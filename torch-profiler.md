## torch使能代码
```
prof = torch.profiler.profile(activities=[torch.profiler.ProfilerActivity.CPU,
                                        torch.profiler.ProfilerActivity.CUDA],
                                        schedule=torch.profiler.schedule(skip_first=1, wait=1, warmup=1, active=2, repeat=3),
                                        on_trace_ready=torch.profiler.tensorboard_trace_handler('profile'),
                                        profile_memory=True,
                                        with_stack=True,
                                        record_shapes=True)
                                        
prof.start()
for epoch in range(args.epochs):
    output = model(input)
    prof.step()
prof.stop()
```
## 查看结果
生成的trace的json文件目前没有events文件，不能再tensorboard上显示
* https://docs.pytorch.org/tutorials/intermediate/tensorboard_profiler_tutorial.html 这个页面提示用chrome://tracing/或者访问https://ui.perfetto.dev/
* 对于chrome://tracing/根据chrome的提示使能就好了
* 关于文件大小的限制，之前tensorboard有限制。在https://ui.perfetto.dev/之前没有发现限制。在chrome://tracing/是否有限制待验证。
