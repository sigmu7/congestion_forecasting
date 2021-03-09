# congestion_forecasting
Repo for forecasting highway traffic congestion using Deep RNNs (LSTM, GRU)

## TL,DR
Forecasting congestion ahead of time is a very important aspect in not only transportation system management, but also a key input to more modern use cases such as ETA estimation for app-based services (Uber/Lyft, Doordash or Grubhub deliveries). Congestion prediction involves using past observations of congestion – using metrics such as speed, travel time – to predict the observations in future. In this study, congestion prediction was performed using deep learning recurrent neural networks. Various architectures of long short-term memory (LSTM) and gated recurrent unit (GRU) were evaluated for their performance. The performance was compared with baseline methods such as vector autoregression (VAR) and historical average.  Experiments with number of units per layer, number of layers, optimizers, learning rate and lengths of sequence input were performed. Experiments are also performed on a smaller subset of sensors to incorporate spatial / network information into the forecasting process using convolutional LSTM architecture. 

![](/images/applications_congestion_prediction.png)

## Introduction
Traffic congestion results is lost time for travelers, extra fuel consumption and low speed and idling vehicles cause air pollution. 2019 Urban Mobility Report states that traffic congestion in the US has resulted in loss of 54 hours per commuter, $166 billion on the whole and wasted 3.3 billion gallons of fuel (Schrank et al. 2019). Lack of proper information of congestion can also result in increased delays, accidents and further congestion due to uninformed travel choices. 

The impact of congestion can be mitigated using advanced information provision to navigation systems and also to highway information message signs. Congestion prediction can also provide insight into analysis of congestion propagation and identification of traffic flow bottlenecks, which can help in strategizing and formulating solutions. The need to use statistical or machine learning models to predict or analyze congestion is that traffic flow is highly stochastic. There are many confounding factors such as road conditions, time of day variability, special events, weather, and also inherent variability of human decision making.

## Data 
Data source for this project is average speed collected by sensors installed on highways in California – a system known as the Performance Measurement System (PeMS) (Caltrans, 2019). The dataset consists of average speed records for 207 sensors for the duration between February and June 2012 collected at a 5-minute interval. These sensors are located on sections of highways in Los Angeles County, namely, I-5, US-101, CA-170, CA-134, CA-2, CA-110.
![](/images/Study_Area.png)
