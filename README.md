# MLOps Roadmap

**Skill Pyramid**

```txt
        LLMs / Advanced DL
     Training Systems & GPUs
        Deep Learning
    Machine Learning + Math
   Data Structures + Python
         Python Basics
```

## Machine Learning Classification

1. **Supervised Learning**: ***Uses labeled data(input + correct output)***
    - Classification -> It predicts discrete categories: email spam/not spam, image cat/dog
    - Regression -> It predicts continuous valuse: house prices, tomorrow's temperature
2. **Unsupervised Learning**: ***Uses unlabeled data; model finds patterns by itself.***
    - Clustering -> Grouping similar data points using distance/similarity metrics; discovers inherent structure without labels.
    - Dimensionality reduction -> Compressing high-dimensional data to lower dimensions; removes redundancy/noise while preserving essential variance/infromation.
    - Anomaly detection -> Identifying rare items/events that deviate significantly from majority patterns; models "normal" behavior to flag outliers.
3. **Semi-Supervised Learning**: ***Uses a small amount of labeled data + a large amount of unlabeled data***
    - Medical imaging -> few expert-labeled scans (e.g. tumors) + many unlabeled scans
    - Speech recognition -> limited transcribed audio + vast amount of raw speech data
    - NLP tasks -> sentiment analysis or text classification with few labeled documents.
4. **Self-Supervised Learning**: ***Creates labes from the data itself(no human labeling)***
    - Constractive learning
    - Masked language modeling(BERT)
    - Masked image modeling
5. **Reinforcement Learning(RL)**: ***An agent learns by interacting with an envrionment, receiving rewards or penalties***
    - Game AI - AlphaGo
    - Robotics - Robot arm control, walking robots
    - Recommendation systems - Real-time ranking/adjustment based on user clicks/dwell time.


##  