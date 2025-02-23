# Prediction-of-battery-life-of-lithium-ion-cell #
 Given various measurements of a Li-ion battery during a limited amount of charging cycles,predict how many cycles has a battery cell lived through and how many cycles will it last before it breaks.

## Motivation for the Project ##

## Dataset Used ##
  The dataset used is [*Randomized Battery Usage Data Set*](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/publications/#batteryrnddischarge) published by the NASA . A comprehsive dataset consisting data of commercial 28 lithium ion cells cycled under different types of conditions with widely varying cycleslives of 250-900 cycles was generated.
  
  Although the dataset contains only 28 cells data but it is still the most diverse dataset out there available freely for use.
  
  The data for each cell included:
  1. Each data is available in two forms which is **MATLAB** and R
  2. **Comments** and **type** includes  charging policy and charging type respectively .
  3. **Cycle data** includes information for each cycle of each cell, and within each cycle,1000s of data points including time, relative time,voltage, current, , temperature.

## Data Preprocessing ##
We first count valid cycles in the dataset as rest after rest is not a cycle .

After that another problem is with cycle type some were reference and most of them were randomized cycles . 
So after this we created our own four parameters cycle_X,battery_count,current and time. cycle_X contain parameters like voltage ,current ,temperature.

## Further Processing the data into input form ##
The framework used for the training was Tensorflow 2.0 with keras as backend.

We decided to perform a deep learning approach using LSTM to train the model.

The dataset was further converted in a format compatible with the inputs for LSTM.For this we need to covert it to sequential time series form which is required for the future prediction.

We have done some tasks like compressing the datset to reduce the training time and limiting zeros,unifying dataset for further use. Also prepare parameters of predicted values.

Another problem is that the parameters ranges different so we need to **Normalise** data and **denormalise** the predicted data.

## *Structure* of the Model ##
Functional API of keras was used, for building up the model layer by layer ,as it required multiple layers which is shown as follows.

In the model we use two lstm layers initially and then 3 dense layers reducing the dimension to predict capacity. In  the we use *selu* actiation with the l2 regularization. And we Mask the input parameters initially.

To boost the gradient desecent we use Adam Optimizer . In the overall model there are 1,24,801 parameters.

- ![](/images/Model.png)

## Training and Validation ## 
The preprocessed dataset was trained with model created. Various hyperparameters were used like learning rate,batch size and epoches etc.

Best accuracy was achieved with 500 epochs, 0.3, 0.000001 learning rate, batch size of 32 along with l2 rate 0.0002 .

We train our model on 20 cells data which include validation also and use 4 cells data for testing .Training loss was 0.0214.

- ![](/images/loss_image.png)

## Testing And Prediction results ##

The model was tested against 4 cells RW11,RW15,RW24 and RW28.
It was tested on losses like mae,mse,rmse,etc.

Dataset | MSE | MAE | RMSE
| :---: | :---: | :---: | :---:
RW-11  | 0.001268 | 0.02864 | 0.03561
RW-15  | 0.007129 | 0.06200 | 0.08443
RW-24  | 0.007129 | 0.03405 | 0.04597
RW-28  | 0.001337 | 0.02697 | 0.03657

We can reduce the losses by increasing the epochs (currently at 500 ) to 700 or 900.


## Future Goals ##
Goals are as follows;
* Reducing the loss by increasing training.
* Adding models like autoencoder to improve the results.
* To use the results obtained to predict realtime life-time for various Li-ion batteries, including the phone and laptop batteries and Electric vehicles.

## Requirements ##
* Tensorflow
* Pandas
* Sklearn
* Scipy
* Ploty
* Jupyter Notebook

## Contributors ##
* [Keshatwar Pratham Naresh](https://github.com/Prathamisok)
* Arsh Jindal
* Arav Khandelwal
* Prakhar Pratap Singh

