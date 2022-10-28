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
How the original video data are collected and processed are described in our repo for [Life-lessons dataset]().

### Raw gesture tokens
The raw gesture tokens are in the folder `data/youtube-video_gesture/label3x3/`, within which each `.txt` file corresponds to one video. 

For instance, the first few lines of `0iApML4l0lI.txt` are as follows:
```
72 45 72 72 72 72 72 72 72 72 72
72 64 56 63 63 63
56 56 64 56 56 64
63 63 63 64 63 63 63
63 72 64 64
...
```

Each line corresponds to one line of utterance in the automatically transcribed subscript from YouTube's API. Each gesture token (e.g., `72`, `45`, etc.) corresponds to one word token.

You can find this one-to-one mapping more clearly in the same file (`0iApML4l0lI.txt`) under the folder `data/youtube-video_text_gesture/label3x3`:
```
if you want to be cast in a movie or a	72 45 72 72 72 72 72 72 72 72 72
television show it's really important to	72 64 56 63 63 63
exactly how the casting process works	56 56 64 56 56 64
because the audition is not the only	63 63 63 64 63 63 63
step in the process	63 72 64 64
...
```
Here you can find that the mapping between a word and a gesture: "if" => 72, "you" => 45, "want" => 72, ... These data are not directly used in the paper, but will be useful in future studies. 

However, note that these are raw data that need be further processed to generate the train/test data used by the models. 

### Generate train/test data
Use the notebook files in folder `preprocess_notebooks` to generate the train/test data.

- Use `prepare_data.ipynb` to convert the raw gesture tokens of each video to one line and collect them to a set. The train-test split ratio is 80%-20%, that is, 10 out of the 53 videos are used for testing.

- As the result, `data/gesture/train.txt` contains 43 lines, and `data/gesture/test.txt` contains 10 lines.

- Use `compress_gestures.ipynb` to generate compressed gesture data from the previous step. That is:
  - `` => 
  - `` => 


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

