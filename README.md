# Parte II: puesta en producción (30%)

* Basado en: 

https://github.com/tenoglez/Proyecto_Modulo4_UEM_2021_TRAIN
https://github.com/tenoglez/Proyecto_Modulo4_UEM_2021_PREDICT


## URLS Training: 
 home: https://mod-4-parte-ii-train-tired-cat-zo.mybluemix.net/
 
 train: https://mod-4-parte-ii-train-tired-cat-zo.mybluemix.net/train-model

## URLS Predict:
 home: https://mod-4-parte-ii-predict-shiny-kookaburra-nm.mybluemix.net/
 
 predict: https://mod-4-parte-ii-predict-shiny-kookaburra-nm.mybluemix.net/predict
 
 Post request para predict:
 
 ```
 curl -X POST -H "Content-Type: application/json" --data '[[10,2,"Nasser, Mrs. Nichols","female",12,1,0,"237736",30.0708, null,"C"]]'  "https://mod-4-parte-ii-predict-shiny-kookaburra-nm.mybluemix.net/predict"
 ```
