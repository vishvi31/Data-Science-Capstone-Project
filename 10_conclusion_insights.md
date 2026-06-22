# Conclusion, Insights and Innovations

Author: Vishvi

---

## Conclusion

This project successfully predicted Falcon 9 first stage
landing outcomes using machine learning classification.

### Key Findings

| Finding | Detail |
|---|---|
| Best ML model | SVM / Logistic Regression / KNN (all 83.3%) |
| Best launch site | KSC LC-39A (77% success rate) |
| Best orbit | ES-L1, GEO, HEO, SSO (100% success) |
| Payload sweet spot | 2000-5500 kg for highest success |
| Success trend | Improved dramatically post-2016 |
| Lowest success orbit | GTO (only 50%) |

---

## Innovative Insights

1. **Time is the strongest predictor**
   SpaceX got dramatically better at landing boosters over time.
   Post-2016, success rates jumped from under 50% to over 80%.
   This suggests engineering learning curves matter more than
   any single technical feature.

2. **Orbit type matters more than payload mass**
   GTO missions fail more often not because of weight, but because
   of the extreme velocity needed for geostationary orbit,
   leaving less fuel for the return burn.

3. **All three top models tied at 83.3%**
   This suggests we have hit a ceiling with the current features.
   Adding weather data, booster age, and fuel margins could
   push accuracy beyond 90%.

4. **KSC LC-39A is SpaceX's most reliable pad**
   Its proximity to ocean landing zones and dedicated
   infrastructure makes it the preferred high-success site.

---

## Creative Additions Beyond the Template

1. Custom colour-coded Folium map with success rate per site
2. Yearly trend line chart showing SpaceX learning curve
3. Business cost interpretation added to introduction
4. Interview Q&A section documenting key decisions
5. Model card documenting limitations and ethical considerations

---

## What I Would Do Next

- Add weather data at launch time as a feature
- Try XGBoost and Random Forest for potentially higher accuracy
- Build a live Flask API that predicts landing success in real time
- Add SHAP values to explain individual predictions
- Collect more recent 2023-2026 launch data via the API

---

*Built by Vishvi | IBM Data Science Professional Certificate Capstone*
*github.com/vishvi31*
