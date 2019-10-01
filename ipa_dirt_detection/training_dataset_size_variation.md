# Training with various sizes of the dirt dataset

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Training with various sizes of the dirt dataset](#training-with-various-sizes-of-the-dirt-dataset)
	- [Summary](#summary)
	- [With backbone darknet53](#with-backbone-darknet53)
		- [Training dataset with 1156 images](#training-dataset-with-1156-images)
		- [Training dataset with 4624 images](#training-dataset-with-4624-images)
		- [Training dataset with 9248 images](#training-dataset-with-9248-images)
		- [Training dataset with 23120 images](#training-dataset-with-23120-images)
		- [Training dataset with 46240 images](#training-dataset-with-46240-images)
	- [With backbone mobilenetv2](#with-backbone-mobilenetv2)
		- [Training dataset with 1156 images](#training-dataset-with-1156-images)
		- [Training dataset with 4624 images](#training-dataset-with-4624-images)
		- [Training dataset with 9248 images](#training-dataset-with-9248-images)
		- [Training dataset with 23120 images](#training-dataset-with-23120-images)
		- [Training dataset with 46240 images](#training-dataset-with-46240-images)

<!-- /TOC -->

## Summary
<escape>
<table>
  <tr>
    <th colspan="2"></th>
    <th colspan="3">mAP - all</th>
    <th colspan="3">mAP - dirt</th>
    <th colspan="3">mAP - object</th>
  </tr>
  <tr>
    <th>Backbone</th>
    <th>Dataset size</th>
    <th>Syn_train</th>
    <th>Syn_test</th>
    <th>Test</th>
    <th>Syn_train</th>
    <th>Syn_test</th>
    <th>Test</th>
    <th>Syn_train</th>
    <th>Syn_test</th>
    <th>Test</th>
  </tr>
  <tr>
    <td rowspan="5">Darknet53</td>
    <td rowspan="1">1156</td>
    <td>81.38</td>
    <td>81.61</td>
    <td>83.65</td>
    <td>75.32</td>
    <td>74.84</td>
    <td>72.06</td>
    <td>87.44</td>
    <td>88.37</td>
    <td>92.13</td>
  </tr>
  <tr>
    <td rowspan="1">4624</td>
    <td>82.41</td>
    <td>83.19</td>
    <td>85.13</td>
    <td>79.16</td>
    <td>79.63</td>
    <td>79.78</td>
    <td>85.67</td>
    <td>86.74</td>
    <td>90.48</td>
  </tr>
  <tr>
    <td rowspan="1">9248</td>
    <td>83.94</td>
    <td>84.59</td>
    <td>85.49</td>
    <td>80.48</td>
    <td>81.00</td>
    <td>78.94</td>
    <td>87.41</td>
    <td>88.18</td>
    <td>92.05</td>
  </tr>
  <tr>
    <td rowspan="1">23120</td>
    <td>84.46</td>
    <td>84.63</td>
    <td>84.14</td>
    <td>80.87</td>
    <td>80.92</td>
    <td>77.60</td>
    <td>88.06</td>
    <td>88.34</td>
    <td>90.68</td>
  </tr>
  <tr>
    <td rowspan="1">46240</td>
    <td>82.14</td>
    <td>82.98</td>
    <td>83.19</td>
    <td>78.90</td>
    <td>79.57</td>
    <td>75.50</td>
    <td>85.38</td>
    <td>86.40</td>
    <td>90.87</td>
  </tr>
  <tr>
    <td rowspan="5">Mobilenetv2</td>
    <td rowspan="1">1156</td>
    <td>78.14</td>
    <td>78.91</td>
    <td>79.60</td>
    <td>71.19</td>
    <td>71.60</td>
    <td>70.59</td>
    <td>85.08</td>
    <td>86.22</td>
    <td>88.60</td>
  </tr>
  <tr>
    <td rowspan="1">4624</td>
    <td>79.72</td>
    <td>82.26</td>
    <td>82.20</td>
    <td>73.83</td>
    <td>77.04</td>
    <td>75.36</td>
    <td>85.61</td>
    <td>87.48</td>
    <td>89.04</td>
  </tr>
  <tr>
    <td rowspan="1">9248</td>
    <td>79.43</td>
    <td>81.62</td>
    <td>83.83</td>
    <td>72.95</td>
    <td>75.33</td>
    <td>77.84</td>
    <td>85.91</td>
    <td>87.91</td>
    <td>89.80</td>
  </tr>
  <tr>
    <td rowspan="1">23120</td>
    <td>80.65</td>
    <td>81.92</td>
    <td>82.04</td>
    <td>74.59</td>
    <td>76.16</td>
    <td>73.62</td>
    <td>86.72</td>
    <td>87.68</td>
    <td>90.47</td>
  </tr>
  <tr>
    <td rowspan="1">46240</td>
    <td>79.87</td>
    <td>81.34</td>
    <td>82.07</td>
    <td>73.91</td>
    <td>75.91</td>
    <td>73.82</td>
    <td>85.83</td>
    <td>86.76</td>
    <td>90.32</td>
  </tr>
</table>
</escape>

What we can observe from the experiment is:

- we performed affine transformation on each sample, randomly add them to different background and add shadows, each image looks totally different. More representations can help with the training process to achieve higher mAP, but within a certain range. The neural network may learn this property through a certain number of such transformations. Therefore, it is unnecessary to generator endless amount of data.
- A huge amount of data which includes many similar foregrounds, will reduce the effectiveness of the training process. The training loss reduces to a very low level within less than one epoch, which will cause the ineffective parameter updating afterwards. Eventually leads to a worse model performance.
- The test results are very consistent in the real image and the synthetic image with the same samples and floors but different representations. This strongly proves the validity of our synthetic data generator. Instead of collecting many real dirt images and labeling them, a synthetic dataset maybe a better choice. The synthetic dataset also allows us to generate more foregrounds, and more small dirt samples in one image, which will help update the parameters of the neural network more efficiently.
- Even more amazing is that we found the mAP and recall in synthetic test set with training floors are also similar to with test floors and real images. In one hand, this proves the generalization ability of our model, in other hand this allows us to set up a trustworthy validation set with limited floors. The precision in synthetic test set with training floors does better, because it can suppress false detection caused by the background.



Criterion
1. IOU > 50%
2. right classification

Setting
1. SCORE_THRESHOLDS = 0.25
2. SCORE_THRESHOLDM = 0.3
3. SCORE_THRESHOLDL = 0.3
4. IOU_NMS          = 0.1
- The number of image for Syn_train testing is: 3448
- The number of image for Syn_test testing is:  1360
- The number of image for Test testing is:      80


## With backbone Darknet53

### Training dataset with 1156 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 79.85     | 80.08  | 75.32 | 79.96 |
| object | 3448   | 13789   | 87.82     | 88.59  | 87.44 | 88.2  |
| all    | 3448   | 34483   | 83.83     | 84.33  | 81.38 | 84.08 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 73.09     | 79.38  | 74.84 | 76.11 |
| object | 1360   | 5458    | 87.75     | 89.52  | 88.37 | 88.63 |
| all    | 1360   | 13640   | 80.42     | 84.45  | 81.61 | 82.37 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 81.57     | 75.0   | 72.06 | 78.15 |
| object | 80     | 284     | 84.76     | 94.01  | 92.13 | 89.15 |
| all    | 80     | 644     | 83.17     | 84.51  | 82.09 | 83.65 |

### Training dataset with 4624 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 78.87     | 83.85  | 79.16 | 81.29 |
| object | 3448   | 13789   | 89.48     | 87.44  | 85.67 | 88.45 |
| all    | 3448   | 34483   | 84.17     | 85.64  | 82.41 | 84.87 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 64.72     | 84.67  | 79.63 | 73.36 |
| object | 1360   | 5458    | 89.57     | 88.26  | 86.74 | 88.91 |
| all    | 1360   | 13640   | 77.14     | 86.46  | 83.19 | 81.13 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 73.17     | 83.33  | 79.78 | 77.92 |
| object | 80     | 284     | 85.95     | 92.61  | 90.48 | 89.15 |
| all    | 80     | 644     | 79.56     | 87.97  | 85.13 | 83.54 |

### Training dataset with 9248 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 77.64     | 84.42  | 80.48 | 80.89 |
| object | 3448   | 13789   | 88.98     | 88.82  | 87.41 | 88.9  |
| all    | 3448   | 34483   | 83.31     | 86.62  | 83.94 | 84.89 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 65.71     | 84.91  | 81.0  | 74.08 |
| object | 1360   | 5458    | 88.84     | 89.67  | 88.18 | 89.25 |
| all    | 1360   | 13640   | 77.27     | 87.29  | 84.59 | 81.67 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 71.9      | 83.89  | 78.94 | 77.44 |
| object | 80     | 284     | 84.71     | 93.66  | 92.05 | 88.96 |
| all    | 80     | 644     | 78.31     | 88.78  | 85.49 | 83.2  |

### Training dataset with 23120 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 80.11     | 84.41  | 80.87 | 82.2  |
| object | 3448   | 13789   | 87.13     | 89.51  | 88.06 | 88.3  |
| all    | 3448   | 34483   | 83.62     | 86.96  | 84.46 | 85.25 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 67.49     | 84.71  | 80.92 | 75.13 |
| object | 1360   | 5458    | 87.24     | 89.79  | 88.34 | 88.5  |
| all    | 1360   | 13640   | 77.37     | 87.25  | 84.63 | 81.81 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 75.65     | 81.11  | 77.6  | 78.28 |
| object | 80     | 284     | 82.81     | 93.31  | 90.68 | 87.75 |
| all    | 80     | 644     | 79.23     | 87.21  | 84.14 | 83.02 |

### Training dataset with 46240 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 81.13     | 84.38  | 78.9  | 82.72 |
| object | 3448   | 13789   | 91.51     | 87.05  | 85.38 | 89.23 |
| all    | 3448   | 34483   | 86.32     | 85.71  | 82.14 | 85.97 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 75.79     | 84.8   | 79.57 | 80.04 |
| object | 1360   | 5458    | 91.89     | 87.96  | 86.4  | 89.88 |
| all    | 1360   | 13640   | 83.84     | 86.38  | 82.98 | 84.96 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 78.79     | 79.44  | 75.5  | 79.11 |
| object | 80     | 284     | 85.71     | 92.96  | 90.87 | 89.19 |
| all    | 80     | 644     | 82.25     | 86.2   | 83.19 | 84.15 |

## With backbone MobileNetV2

### Training dataset with 1156 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 78.85     | 76.41  | 71.19 | 77.61 |
| object | 3448   | 13789   | 83.35     | 87.58  | 85.08 | 85.41 |
| all    | 3448   | 34483   | 81.1      | 82.0   | 78.14 | 81.51 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 74.45     | 76.92  | 71.6  | 75.67 |
| object | 1360   | 5458    | 82.82     | 88.95  | 86.22 | 85.78 |
| all    | 1360   | 13640   | 78.64     | 82.94  | 78.91 | 80.72 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 76.07     | 74.17  | 70.59 | 75.11 |
| object | 80     | 284     | 80.18     | 92.61  | 88.6  | 85.95 |
| all    | 80     | 644     | 78.13     | 83.39  | 79.6  | 80.53 |

### Training dataset with 4624 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 80.38     | 78.43  | 73.83 | 79.39 |
| object | 3448   | 13789   | 86.2      | 87.63  | 85.61 | 86.91 |
| all    | 3448   | 34483   | 83.29     | 83.03  | 79.72 | 83.15 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 73.79     | 81.35  | 77.04 | 77.39 |
| object | 1360   | 5458    | 88.35     | 89.3   | 87.48 | 88.82 |
| all    | 1360   | 13640   | 81.07     | 85.32  | 82.26 | 83.1  |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 75.34     | 78.06  | 75.36 | 76.67 |
| object | 80     | 284     | 82.65     | 92.25  | 89.04 | 87.19 |
| all    | 80     | 644     | 78.99     | 85.15  | 82.2  | 81.93 |

### Training dataset with 9248 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 80.28     | 78.14  | 72.95 | 79.2  |
| object | 3448   | 13789   | 86.57     | 87.81  | 85.91 | 87.18 |
| all    | 3448   | 34483   | 83.43     | 82.97  | 79.43 | 83.19 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 76.12     | 80.02  | 75.33 | 78.02 |
| object | 1360   | 5458    | 88.19     | 89.65  | 87.91 | 88.92 |
| all    | 1360   | 13640   | 82.16     | 84.83  | 81.62 | 83.47 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 81.59     | 80.0   | 77.84 | 80.79 |
| object | 80     | 284     | 84.03     | 92.61  | 89.8  | 88.11 |
| all    | 80     | 644     | 82.81     | 86.3   | 83.82 | 84.45 |

### Training dataset with 23120 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 80.23     | 79.0   | 74.59 | 79.61 |
| object | 3448   | 13789   | 87.03     | 88.11  | 86.72 | 87.57 |
| all    | 3448   | 34483   | 83.63     | 83.56  | 80.65 | 83.59 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 74.45     | 80.41  | 76.16 | 77.31 |
| object | 1360   | 5458    | 87.18     | 89.25  | 87.68 | 88.2  |
| all    | 1360   | 13640   | 80.82     | 84.83  | 81.92 | 82.76 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 80.64     | 77.5   | 73.62 | 79.04 |
| object | 80     | 284     | 83.54     | 92.96  | 90.47 | 88.0  |
| all    | 80     | 644     | 82.09     | 85.23  | 82.04 | 83.52 |

### Training dataset with 46240 images

Result in synthetic test with training floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 3448   | 20694   | 80.06     | 78.08  | 73.91 | 79.05 |
| object | 3448   | 13789   | 86.44     | 87.6   | 85.83 | 87.02 |
| all    | 3448   | 34483   | 83.25     | 82.84  | 79.87 | 83.03 |

Result in synthetic test with test floor

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 1360   | 8182    | 74.56     | 79.83  | 75.91 | 77.11 |
| object | 1360   | 5458    | 86.91     | 88.57  | 86.76 | 87.73 |
| all    | 1360   | 13640   | 80.73     | 84.2   | 81.34 | 82.42 |

Result in real test

| Class  | Images | Targets | Precision | Recall | mAP   | F1    |
| ------ | ------ | ------- | --------- | ------ | ----- | ----- |
| dirt   | 80     | 360     | 79.25     | 76.39  | 73.82 | 77.79 |
| object | 80     | 284     | 83.02     | 92.96  | 90.32 | 87.71 |
| all    | 80     | 644     | 81.13     | 84.67  | 82.07 | 82.75 |
