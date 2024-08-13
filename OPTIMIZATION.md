# Optimizations

The SNPE SDK comes with a bunch of optimization plans inbuilt for running DLC models on Qualcomm hardware. Some considerations when trying to optimize performance without altering the exisiting DLC model:
- Utilizing UserBuffer to avoid additional copies of data in/out of SNPE SDK by directly reading and writing to the buffers directly
- It is more efficient to move data in/out of tensors than to use iterators.
- Tensors must be manipulated in a UserBuffer before/after going into a network.
- For Tensorflow based models, optimize Tensorflow graphs prior to generating a DLC file
- Set the performance profile to HIGH_PERFORMANCE when executing DLC files
- Disable profiling information in production environment
- Smaller networks may run faster on CPU than GPU due to GPU memory overheads.
- Run input preprocessing steps like scaling, conversions, crop etc prior to passing inputs to SNPE SDK when running on DSP. They are not optimized to run on DSP runtime.
- For DSP V68 version and above, enabling the init cache mode is recommended.

Considerations when we also want to optimize the DLC file itself:
- Run quantized quantized fixed-point model on the CPU for better performance at the cost of accuracy using `snpe-net-run --container <path_to_quantized_dlc> --input_list <path_to_input_list> --enable_cpu_fxp`
- Utilize offline graph caching to reduce initialization time when executing on HTP using `snpe-dlc-graph-prepare`
- Quantize DLC model weights using `snpe-dlc-quantize` and use combination of flags `--optimizations`, `--algorithms`, `--param_quantizer`, `--act_quantize`, `--use_per_channel_quantization`, `--use_per_row_quantization`

