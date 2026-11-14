# Airbnb Price Prediction

Final project for ITI. We built a regression model that predicts the price of an Airbnb listing in New York City from its details (location, room type, capacity, amenities and reviews).

**Team:** *(add the team members' names here)*

## Project steps

All the steps are in `notebook.ipynb`:

1. **Cleaning**: dropped columns we didn't need, handled missing values, removed 224 duplicate rows and fixed data types. About 21,498 listings were left.
2. **EDA**: price is heavily right-skewed, so we used `log1p(price)` as the target. The strongest numeric feature for price was `accommodates`, and room type also matters a lot. Review features had only a weak correlation with price.
3. **Preprocessing**: encoding, log transform of the target, train/test split and scaling.
4. **Modeling**: trained 6 regression models and compared them.
5. **Data size experiment**: re-trained the best 4 models on 1%, 10% and 100% of the training data.
6. **Tuning**: tuned Random Forest with cross-validated search and compared the results before and after.

## Results

Test set results (the target is log price, so the errors are in log units):

| Model | MAE | RMSE | R² |
|-------|-----|------|----|
| Linear Regression | 0.409 | 0.546 | 0.565 |
| Decision Tree | 0.324 | 0.462 | 0.689 |
| Random Forest | 0.264 | 0.379 | 0.790 |
| XGBoost | 0.267 | 0.379 | 0.791 |
| SVM | 0.290 | 0.415 | 0.749 |
| **CatBoost** | **0.258** | **0.364** | **0.807** |

CatBoost gave the best score. We looked closer at Random Forest because it was almost as good and easier to explain. Tuning it barely changed the test score (R² 0.790 before, 0.790 after). The train R² went down a bit (0.970 to 0.965), so the tuned model memorizes the training data a little less.

The full tables are in the `results` folder.

## How to run

1. Clone the repo and install the libraries:
   ```
   pip install -r requirements.txt
   ```
2. Download the NYC `listings.csv` file from [Inside Airbnb](https://insideairbnb.com/get-the-data/) and put it next to the notebook. It is not in the repo because of its size.
3. The notebook was written in Google Colab and reads the data from `/content/listings.csv`. If you run it on your own computer, change that path to `listings.csv`.
4. The last cells save the models into a `deployment_assets` folder. The Random Forest files are more than 300 MB each, so they are not uploaded here.

## Files

```
notebook.ipynb          full code, from cleaning to tuning
results/                model comparison tables (csv)
docs/presentation.pptx  our final presentation
requirements.txt        python libraries
```

## Tools

Python, pandas, NumPy, scikit-learn, XGBoost, CatBoost, Plotly, Matplotlib, Seaborn
