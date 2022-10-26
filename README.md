# COLING2022-RationalGestures
Code for reproducing the results in paper: [**Xu, Y., Cheng, Y., & Bhatia, R. (2022, October). Gestures Are Used Rationally: Information Theoretic Evidence from Neural Sequential Models. In *Proceedings of the 29th International Conference on Computational Linguistics* (pp. 134-140).**](https://aclanthology.org/2022.coling-1.12/)

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

## Data preparation
How the original video data are collected and processed are described in the repo for [Life-lessons dataset].

## Train the models
Run commands as follows:
```
python run_[MODEL_NAME]_[DATA_NAME].py --config config/[MODEL]_[DATA_NAME].yaml --data data/[DATA_NAME] --log_file LOG_FILE_NAME
```
Models will be saved to the `best_[MODEL_NAME]_[DATA_NAME].pt` file. The validation loss during training will be saved to `LOG_FILE_NAME`, which will be used to produce Fig. in the paper.

For example, you can run the command:
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

Assume that you have trained the models (LSTM or Transformer), you can run the following command to obtain the perplexity scores:

```
python run_[MODEL_NAME]_[DATA_NAME].py --config config/[MODEL]_[DATA_NAME].yaml --data data/[DATA_NAME] --task compute --load MODEL_PATH --output_ppl OUTPUT_PREFIX
```

For example, after you run the command:
```
python run_lstm_gesture.py --config config/lstm_gesture.yaml --data data/gesture --task compute --load best_lstm_gesture_gesture.pt --output_ppl lstm_gesture_ppls
```

Two output files will be created: `lstm_gesture_ppls_test.txt` and `lstm_gesture_ppls_train.txt`, which contain perplexity scores for sentences from the test and train set, respectively. The content looks like this:
```
0,1.5285775478963592,0
0,1.0480365517994947,1
0,1.497528041103122,2
0,1.312197618269433,3
...
```
The three columns are *line index*, *ppl score*, and *position*. Fig. 4 in the paper is produced using the last two columns, i.e., ppl agains position. 

