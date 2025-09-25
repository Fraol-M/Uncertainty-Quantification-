# Uncertainty Quantification in Deep Learning

This project implements and compares three methods for estimating uncertainty in a deep learning model trained on the MNIST dataset.

## Methods Implemented

1. **Monte Carlo Dropout (MC Dropout)**  
   - Applies dropout during inference and performs multiple forward passes to obtain a distribution of predictions.

2. **Deep Ensembles**  
   - Trains multiple independent models and averages their predictions.

3. **Temperature Scaling**  
   - Calibrates the output probabilities of a single, trained model by scaling the logits with a learned temperature parameter.

## Observations and Analysis

### Predictions
- The Ensemble method achieved the highest accuracy on the test subset.  
- MC Dropout and Temperature Scaling performed closely behind.  
- All methods performed well overall.

### Uncertainty Estimates
- MC Dropout and Deep Ensembles showed higher uncertainty for incorrect predictions, indicating their ability to capture predictive uncertainty.  
- The distribution of uncertainty estimates differed between MC Dropout and Ensembles, as observed in histograms.

### Calibration
- The base model was overconfident.  
- Both Temperature Scaling and Deep Ensembles significantly improved calibration, bringing predicted probabilities closer to the true fraction of positives.  
- Temperature Scaling specifically focuses on calibration without altering the rank of predictions.

## Computational Cost

| Method                  | Training Cost            | Inference Cost                                     |
|-------------------------|------------------------|--------------------------------------------------|
| Base Model              | Low                     | Low                                              |
| Temperature Scaling     | Base model + small optimization | Similar to base model                          |
| MC Dropout              | Base model with dropout | Multiple forward passes (T=50), increases inference time |
| Deep Ensembles          | Train multiple models (5) | Run each model and average results, highest inference cost |

## Method Suitability

- **High accuracy & reliable uncertainty:** Deep Ensembles provide the best performance with meaningful uncertainty estimates and good calibration, but are computationally expensive.  
- **Capturing uncertainty with moderate cost:** MC Dropout is a good balance, requiring only a single model with dropout and increased inference time.  
- **Improving calibration of an existing model:** Temperature Scaling is simple, effective, and computationally cheap during inference. Ideal for pre-trained models when only calibrated probabilities are needed.

## Conclusion

The choice of uncertainty estimation method depends on the specific requirements and available resources:

- **Deep Ensembles:** Best performance, high cost.  
- **MC Dropout:** Good balance between performance and cost.  
- **Temperature Scaling:** Effective calibration with minimal computation.
