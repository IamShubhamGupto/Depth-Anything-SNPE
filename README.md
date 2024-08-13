# Depth-Anything-SNPE
Running Depth Anything model on Qualcomm SNPE SDK.

## Generate ONNX model

We will be using [Depth-Anything-ONNX](https://github.com/fabio-sim/Depth-Anything-ONNX/tree/main) to generate ONNX weight files of the model. 

### Environment setup
```
# clone into Depth-Anything-ONNX
conda create -n dav2 python=3.11
conda activate dav2
pip install -r requirements.txt
```
### Generation
```
python dynamo.py export --encoder vits -o depth_anything_vits_op16.onnx -b 1 --opset 16
```

## Running SNPE SDK
This step assumes you have installed SNPE SDK successfully. You should see the following message if sourced correctly.
```
[INFO] AISW SDK environment set
[INFO] QNN_SDK_ROOT: /mnt/d/Downloads_F/v2.22.6.240515/qairt/2.22.6.240515
[INFO] SNPE_ROOT: /mnt/d/Downloads_F/v2.22.6.240515/qairt/2.22.6.240515
SDK environment now set up; additionally you may now run devtool to perform development tasks.
Run devtool --help for further details.
```

### Python Environment
```
# clone the repository
conda create -f environment_snpe.yml
conda activate snpe
```

### Generate DLC from ONNX
```
snpe-onnx-to-dlc --input_network assets/onnx/depth_anything_vits_op16.onnx -o assets/dlc/depth_anything_vits_op16.dlc
```

### Run DLC 
```
snpe-net-run --container assets/dlc/depth_anything_vits_op16.dlc --input_list assets/input_list.txt
```

A successful run should give the following output
```
-------------------------------------------------------------------------------
Model String: N/A
SNPE v2.22.6.240515184619_92920
-------------------------------------------------------------------------------

Processing graph : depth_anything_vits_op16
Processing DNN input(s):
./input/input.raw
Processing DNN input(s):
./input/input2.raw
Successfully executed graph depth_anything_vits_op16
```

## Optimizations

Refer to [OPTIMIZATION.md](./OPTIMIZATION.md) for further improving the performance of the DLC model.

## References
- [Qualcomm SNPE SDK](https://docs.qualcomm.com/bundle/publicresource/topics/80-63442-2/introduction.html)