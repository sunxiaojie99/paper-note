**Dense retrieval**

**Finetune**

====

负样本选择

In-Batch Negative：In-Batch Negative is more noise-tolerant than AR2 and HardNegative.

Hard Negative

AR2：adopts anadversarial-training approach to select “hard” negative samples iteratively

===

调参数

conduct grid search over learning-rate in {2e-5, 1e-5}

batchsize in {32, 64, 128}

We report the average results under 3 different random seeds.

