## vllm build from source
### with prebuild wheel
* https://docs.vllm.ai/en/stable/getting_started/installation/gpu.html#install-the-latest-code
```
export VLLM_COMMIT=72d9c316d3f6ede485146fe5aabd4e61dbc59069 # use full commit hash from the main branch
export VLLM_PRECOMPILED_WHEEL_LOCATION=https://wheels.vllm.ai/${VLLM_COMMIT}/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl
pip install --editable .
```
## vllm fully build和不改变torch等
* TODO:  https://docs.vllm.ai/en/stable/getting_started/installation/gpu.html#install-the-latest-code

## Test
```
CUDA_VISIBLE_DEVICES=0,1,2,3 python3 vllm/benchmarks/benchmark_throughput.py --backend vllm --model /lpai/models/mistralai__mixtral-8x7b-v0_1/24-01-21-1035/  --dataset ./sglang/angel/datas/ShareGPT_V3_unfiltered_cleaned_split.json --num-prompts 3000
```

Issue: https://blog.csdn.net/kazoos/article/details/135014114
