# Forecasting Highway Traffic Congestion using Deep Recurrent Neural Network Models

## TL,DR
Forecasting congestion ahead of time is a very important aspect in not only transportation system management, but also a key input to more modern use cases such as ETA estimation for app-based services (Uber/Lyft, Doordash or Grubhub deliveries). Congestion prediction involves using past observations of congestion – using metrics such as speed, travel time – to predict the observations in future. In this study, congestion prediction was performed using deep learning recurrent neural networks. Various architectures of long short-term memory (LSTM) and gated recurrent unit (GRU) were coded and evaluated for their performance in Keras. The performance was compared with baseline methods such as vector autoregression (VAR) and historical average.  Experiments with number of units per layer, number of layers, optimizers, learning rate and lengths of sequence input were performed. Experiments are also performed on a smaller subset of sensors to incorporate spatial / network information into the forecasting process using convolutional LSTM architecture. 

![](/images/applications_congestion_prediction.png)

## Introduction
Traffic congestion results is lost time for travelers, extra fuel consumption and low speed and idling vehicles cause air pollution. 2019 Urban Mobility Report states that traffic congestion in the US has resulted in loss of 54 hours per commuter, $166 billion on the whole and wasted 3.3 billion gallons of fuel (Schrank et al. 2019). Lack of proper information of congestion can also result in increased delays, accidents and further congestion due to uninformed travel choices. 

The impact of congestion can be mitigated using advanced information provision to navigation systems and also to highway information message signs. Congestion prediction can also provide insight into analysis of congestion propagation and identification of traffic flow bottlenecks, which can help in strategizing and formulating solutions. The need to use statistical or machine learning models to predict or analyze congestion is that traffic flow is highly stochastic. There are many confounding factors such as road conditions, time of day variability, special events, weather, and also inherent variability of human decision making.

## Data & Pre-processing
Data source for this project is average speed collected by sensors installed on highways in California – a system known as the Performance Measurement System (PeMS) (Caltrans, 2019). The dataset consists of average speed records for 207 sensors for the duration between February and June 2012 collected at a 5-minute interval. These sensors are located on sections of highways in Los Angeles County, namely, I-5, US-101, CA-170, CA-134, CA-2, CA-110.
![](/images/Study_Area.png)

A sample plot for three sensors shows that the ‘peak’ period, represented by low speed, can occur during different times of the day. Once compiled, the data was normalized. Both the StandardScaler using mean and standard deviation and MinMax scalers available in the Keras API were used.  The MinMaxScaler provided better training results, which was used for all further analyses and experiments.
![](/images/time_series_plot_1.png)

Finally, following the scaling process, several sensor time series were tested for stationarity using unit root test through the Dickey-Fuller test (Dickey and Fuller (1979)). For all the test the p-value for the test was very close to 0. This implies that there is no unit root among any of the series. 
70% of the available time period of data is used for training. 20% for validation and 10% for testing.

## Metrics, Architectures & Hyperparameters Tested

#### Metrics
![](/images/mae.png) ![](/images/rmse.png)

#### Baseline
- VAR
- Historical average
#### Neural Nets
- Feed forward Multi-layer Perceptron (FFMLP)
- GRU (multiple layers & neurons/layer)
- LSTM (multiple layers & neurons/layer)

#### Hyperparameters
- Optimizers (SGD, RMSProp, Adam, Adamax)
- Learning rate
- Input sequence length
- no. of epochs = 40
- batch size = 128
- ReLU activation function

##  Forecast Error Comparison
#### Architecture Comparison w/ MAE
![](/images/rnn_arch_mae_comparison.png)
#### Hyperparameter comparison
![](/images/optim_lr_comparison.png)
#### Architecture Comparison w/ RMSE
![](/images/rnn_arch_rmse_comparison.png)
#### Input Sequence Length Comparison
![](/images/input_seq_length_illustration_mae.png)

#### Compare different models for overall performance
In order to compare the performance of model trained using MAE vs RMSE as loss metrics, the cumulative distribution (CDF) of prediction errors for the better models for different sensors on test data is plotted. The closer each curve of the CDF is to the right edge the worse is the model performance. GRU with 3 layers is worse than all others. The GRU 1 layer with 128 neurons, LSTM layer with 128 neurons are marginally better than all others.
![](/images/pred_error_cumu_dist.png)

#### Comparison of Model Forecasts
![](/images/rnn_pred_err_accident_comparison.png)
The "optimal" model is able to predict the inception of peak period better than other models. Also, in this case, based on the observed data, it appears as if that around time unit 220-225, the peak period is about to finish and the observed speed recovers to free flowing speed. However, at time unit 230, there is another drop in speed. Usually, this happens when there is an accident. The GRU 2-layer models are able to predict this feature, albeit, not as accurately, among which again the optimal model performs better.
Once reason that the optimal model may not detect the second onset of congestion could be because that the architecture is not learning congestion from other sensors. As the neighboring sensors are close to each other, the congestion detected at one sensor could reach the adjacent sensor in some time due to the nature of traffic flow. Thus, there is correlation between speed observed at neighboring sensors. This spatial correlation needs to be exploited and added to the temporal learning of an RNN to obtain predictions with better accuracy.

## Incorporating Spatial Information
Heatmap of congestion shows the propagation of congestion from downstream section of highway to upstream.
![](/images/need_for_spatial_info.png)

Shi et al. (2015) proposed a new architecture called convolutional LSTM (conv-LSTM) that uses information from neighboring spatial observations in addition to temporatl to forecast. The authors used this approach in precipitation forecasting using data from precipitation monitoring stations over a grid. A sliding filter is applied across various stations as a convolution operation. An illustration of conv-LSTM:
![](/images/lstm_convlstm.png)

For the current use case of highway congestion forecasting, conv-LSTM is applied only on one dimension.

#### Forecasting error comparison with spatial network information
![](/images/rnns_vs_convlstm_table.png)
![](/images/rnns_vs_convlstm.png)

