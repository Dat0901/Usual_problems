Issues when use flash attention (https://github.com/Dao-AILab/flash-attention)

**RuntimeError: FlashAttention only supports Ampere GPUs or newer.** \
Flash Attention 2 or higher only support Ampere GPUs (NVIDIA A100, RTX 3090/4090, H100, etc.), while older Turing GPUs (V100, T4, or GTX 10xx/20xx) Flash attention haven't support yet (maybe in the future). \
-> Downgrade to Flash attention 1.x -> **pip install "flass-attn<2.0.0"** 
