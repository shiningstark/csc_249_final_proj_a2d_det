# Actor-Action Detection
## Installation

Please run the following command before running the code

```bash
export PATH=/usr/local/cuda/bin:$PATH
bash mask_rcnn/make.sh
```



## Usage

1. **Training**

   This shell script, `scripts/train.sh`, will call train.py with some command line arguments (e.g., learning rate, training epochs, etc.). You can change the arguments in  the `scripts/train.sh`. For more details of arguments, please run `python train.py --help`.

   To start training, please run:

   ```bash
   bash scripts/train.sh
   ```

2. **Evaluation**

   Step 1: generating detection result

   In `scripts/gen_det_val.sh`, please replace `--load_ckpt` with the checkpoint model (`.pth` file) to be evaluate, which should locate at `--output_dir` given in `scripts/train.sh`. Besides, you may also change the file path given at `--det_result_pkl` .

   ```bash
   bash scripts/gen_det_val.sh
   ```

   Step 2: evaluating the detection result

   ```bash
   python eval/baseline_pascal_voc_map.py \
   	--gt_cls_pkl path_to_detection_grount_truth:small_val_box_by_cls.pkl
   	--det_cls_pkl path_to_the_detection_result \ # generated by step 1 (--det_result_pkl in scripts/gen_det.sh)
   	--mode actor_action # You can use 'actor' or 'action' here to see the mAP when only consider actor labels or action labels
   ```

3. **Generate Result on Testing set**

   It's the similar with step 1 in **evaluation**. Please run

   ```bash
   bash scripts/gen_det_test.sh
   ```

   Please make sure to change the `--load_ckpt` with the new checkpoint model.



## Data Processing

We provide the the code of data loading part. Now it supports loading neighboring frames and forming a segment, which may be helpful to action recognition. Also, optical flow corresponding to the loaded segment will also be computed by Gunnar-Farneback optical flow esimation algorithm.



## Model

The baseline model we choose is Faster-RCNN with Feature Pyramid Network, whose backbone network is ResNet-50. You can switch to other models by using other configuration files in `model_cfgs/` and replace the flag `--cfg` in `scripts/train.sh`. Of course, you can use any other models as long as it achieves better performance.



## Acknowledgement

Thanks to [@roytseng-tw](https://github.com/roytseng-tw) for <https://github.com/roytseng-tw/Detectron.pytorch>

