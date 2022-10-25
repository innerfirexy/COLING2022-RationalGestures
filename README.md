# COLING2022-RationalGestures
Code for reproducing the results in paper: **Xu, Y., Cheng, Y., & Bhatia, R. (2022, October). Gestures Are Used Rationally: Information Theoretic Evidence from Neural Sequential Models. In *Proceedings of the 29th International Conference on Computational Linguistics* (pp. 134-140).**

## Cite
```
@inproceedings{xu2022gestures,
  title={Gestures Are Used Rationally: Information Theoretic Evidence from Neural Sequential Models},
  author={Xu, Yang and Cheng, Yang and Bhatia, Riya},
  booktitle={Proceedings of the 29th International Conference on Computational Linguistics},
  pages={134--140},
  year={2022}
}
```

## Install dependancies

## Train the models
Run commands:
```
python run_[MODEL_NAME]_[DATA_NAME].py --config config/[]_[].yaml --data data/[DATA_NAME] --log_file LOG_FILE_NAME
```
Models will be saved to the `best_[MODEL_NAME]_[DATA_NAME].pt` file. The validation loss during training will be saved to `LOG_FILE_NAME`, which will be used to produce Fig. in the paper.

For example, you can run the following code:
```
python run_lstm_gesture.py --config config/lstm_gesture.yaml --data data/gesture --log_file lstm_gesture_loss.txt
```
It means you have trained an LSTM-based model for predicting gesture sequences, which is saved to `best_lstm_gesture_gesture.pt` in the same directory, and `lstm_gesture_loss.txt` contains the validation loss per epoch that looks like this: 

```
--
epoch,loss
1, 7.40
2, 7.08
3, 6.96
...
20, 6.85
--
```

You can then read it with `pandas` and generate the loss~epoch plot using `matplotlib` or `ggplot2` in R language.

## Produce perplexity scores
