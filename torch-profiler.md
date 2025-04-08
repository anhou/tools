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
