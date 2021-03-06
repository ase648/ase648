# Additional experiments of ASE648, which do not appear on the paper. You can choose to see it or not.
## Experiment Setup
Model: Yolov5

Dataset: COCO

Sample size: 1000

Noise perturbation effect: the rain effect generated by the PyTorch.

The YOLO family is the most popular model for object detection both in academia and industry, and YOLOv5 is the latest YOLO model, which achieves the SOTA performance on the object detection task. YOLO model is usually composed of components such as backbone and classifier, with each component being a typical DNN model such as ResNet.

For YOLOv5, we evaluated the performance of DeepXplore and Themis. The results of other three testing systems are similar to those in DeepXplore for this YOLO model.

<!-- Considering that the performance of various test tools on different models and datasets has been provided in the paper, experiments have shown that DeepXplore, DeepGauge and Supersize have similar performance. Therefore, we only test the performance of DeepXplore and Themis on the yolov5 model. -->
![IMG_00002](https://user-images.githubusercontent.com/96641982/158176617-2e9330cc-bf57-47e7-9de8-0101ef16b50e.jpeg)

<center style="font-size:14px;color:#C0C0C0;text-decoration:underline">Figure 1. Number of faults detected by Themis and DeepXplore when their test coverage metric reaches 100% </center> 


![IMG_00001](https://user-images.githubusercontent.com/96641982/158960237-3cafcd0c-2c9d-4874-9d6f-a9a6277693bf.jpeg)


<center style="font-size:14px;color:#C0C0C0;text-decoration:underline">Figure 2. Accuracy variations of a DLS (YOLOv5) on both a clean COCO dataset and a COCO+noise dataset by retraining the DLS with faults detected by Themis and DeepXplore. </center> 


----------------

**Figure 1** indicates that, when the test coverage metrices of Themis and DeepXplore both reach 100%, Themis generated much more faults than DeepXplore.


In **Figure 2**, we compare the performance of Themis and DeepXplore by 1) retraining the DLS with faults detected by Themis and DeepXplore, 2) testing the mAP accuracies of models' trained by both systems on the original COCO dataset (**TEST 1, Red Bar for Themis, Blue Bar for DeepXplore**), 3) and testing the mAP accuracies of models' trained by both systems on the COCO+noise (generated by PyTorch) dataset (**TEST 2, Green Bar for Themis, Pink Bar for DeepXplore**).


**TEST 1** indicates that, under a clean COCO dataset (e.g., representing a sunny weather without noises), Themis does not decrease the model's accuracy on the COCO dataset, and the repaired accuracy is better than DeepXplore.


**TEST 2** indicates that, under a COCO+noises dataset (e.g., representing a rainy weather with noises), Themis can still achieve a high accuracy (comparable to the original accuracy before repairing), which is 4.9% higher than DeepXplore.


# This is the code for ASE'22 submission
Belows are the commands to obtain the experiment results in the paper.


# convergence analysis
python run.py --dataset mnist --attack rand --diff --adv --samplesize 100
--attack: type of attack [rand/pgd] (required)
--dataset: what dataset [mnist/cifar10] (required)


--model: which model [wideresnet/smallcnn/densenet121] (required)
--eval: evaluate natural accuracy and robustness accuracy/ attack (required)
--train: use training data (by default: False)
if eval:
    --epsilon: noise level (default value set to 0.031)
if not eval but retrain: --retrain
    --epsilon: noise level (default value set to 0.031)
    --num_data: number of faults to retrain (default: 1)
if not eval:
    --diff/--gen: compute divergence and mcmc/ generate data (required)
    if diff:
        --epsilon: noise level (default value set to 0.031)
        --samplesize: number of sample for mcmc (default value set to 100)
        --round: which round of testing now it is (default: 1)
        --heat/--var: output heatmap/variance of divergence (not require)
            if var:
            --row_max: (var) printing the number of rows
    if gen:#for rand attack, no need gen as rand directly transformed input dataset
        --epsilon: noise level (default value set to 0.031)

python run.py --attack rand --dataset cifar10 --model wideresnet --gen --epsilon 0.001
python run.py --attack rand --dataset cifar10 --model wideresnet --eval --epsilon 0.001
python run.py --attack pgd --dataset cifar10 --model wideresnet --retrain --epsilon 0.01 --num_data 100
python analyze_mcmc.py --mode mcmc --path metadata/1/mcmc_info_cifar10_densenet121_pgd_1000_0.05.txt --t 0.0

# massively gen dataset, retrain and evaluate
gen_acc.py 
--mode: which mode [gen/retrain/eval_original/eval_original] (required)
--path: which file path (for gen_csv)
python analyze_mcmc.py:
--mode: mcmc or robust
if mcmc:
    --path: path that contains multiple test results
    --t: threshold value to identify convergence
elif robust:
    --dataset: which dataset [cifar10/mnist]
    --model: which model [wideresnet/smallcnn]
    --attack: which attack [rand/pgd]
    --epsilon: 0.01
    --samplesize: 100


............
