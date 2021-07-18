## Get started
1. Download preprocessed datasets
```bash
git clone https://github.com/KaiyangZhou/pytorch-vsumm-reinforce
cd pytorch-vsumm-reinforce
```
Then go to this [line](https://drive.google.com/drive/folders/1xiw9-rexKFAIocc4shvwPUqTL2fbpxfC?usp=sharing) to download Summe dataset and frames

2. Make splits
```bash
python create_split.py -d datasets/[name dataset file] --save-dir datasets --save-name [name split file]  --num-splits 5
```
As a result, the dataset is randomly split for 5 times, which are saved as json file.

Train and test codes are written in `main.py`. To see the detailed arguments, please do `python main.py -h`.

## How to train
```bash
python main.py -d datasets/[name dataset file] -s datasets/[name split file].json -m [summe/tvsum] --gpu 0 --save-dir log/[name log split dir] --split-id 0 --verbose
```

## How to test
```bash
python main.py -d datasets/[name dataset file] -s datasets/[name split file].json -m [summe/tvsum] --gpu 0 --save-dir log/[name log split dir] --split-id 0 --evaluate --resume log/[name log split dir]/model_epoch60.pth.tar --verbose --save-results
```

If argument `--save-results` is enabled, output results will be saved to `results.h5` under the same folder specified by `--save-dir`. To visualize the score-vs-gtscore, simple do
```bash
python visualize_results.py -p path_to/result.h5
```

## Plot
We provide codes to plot the rewards obtained at each epoch. Use `parse_log.py` to plot the average rewards
```bash
python parse_log.py -p path_to/log_train.txt
```

## Visualize summary
You can use `summary2video.py` to transform the binary `machine_summary` to real summary video. You need to have a directory containing video frames. The code will automatically write summary frames to a video where the frame rate can be controlled. Use the following command to generate a `.mp4` video
```bash
!python summary2video.py -p log//[name log split dir]/result.h5 -d SumMe/videos/frames -i 1 --fps 30 --save-dir log --save-name summary.mp4
```
