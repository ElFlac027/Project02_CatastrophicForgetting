# Project02_CatastrophicForgetting
Alma Mater Studiorum - Università di Bologna, Corso di Cybersecurity M, anno 2025/2026, Michele Colajanni

Gruppo 20: Sergio Romeo, 0001092962

Progetto numero 2: Adversarial Attack using the Catastrophic Forgetting 

Questo progetto consiste nell'implementazione tramite il framework PyTorch di un modello di **class-incremental learning** con l'obiettivo di studiare il **catastrophic forgetting** in presenza di un **data poisoning attack**, cercando di quantificare il numero di dati avvelenati necessario a ridurre le capacità del sistema.

## Caratteristiche principali

- Modello: Multi-Layer Perceptron (MLP)
- Approccio: Class-Incremental Learning
- Replay Buffer: experience replay, dimensione a default = 4000, campionamento casuale
- Fase 1: Training sequenziale su 5 task (Priorità alle classi meno frequenti, 2 classi per task)
- Fase 2: Training personalizzabile con dati "avvelenati"
  - Perturbazione sulle feature numeriche
  - Label flipping mirato (Es: backdoor → normal)
- Metriche: Accuracy globale + classification report
- Seeds per riproducibilità

## Dataset

- File: `train_test_network.csv`
- Classi e feature sono descritte nei due file PDF presenti nella directory "Dataset_description"
- Preprocessing: one-hot su feature `proto` e `conn_state`, standard scaling su tutte le feature, sostituzione `-` → 0
- Split stratificato: 70% train / 30% test

## Sintesi risultati ottenuti
1. Accuracy passa da 87,76% (Fase 1) a 86,69% (Fase 2 - Data Poisoning), degrado generale trascurabile (Configurazione predefinita)
2. Soglie per forgetting totale relativamente basse nel poisoning mirato
3. Replay buffer con dimensione di 4000 offre i migliori risultati (Configurazione predefinita), riducendo la dimensione aumenta il forgetting collaterale
4. I test hanno mostrato come il modello, seppur semplice, abbia una buona resistenza ai tentativi stealth di avvelenamento cumulativo, mentre soffre poisoning più aggressivi


## Requisiti
- Python 3.8+
- PyTorch (torch, torch.nn, torch.optim)
- numpy
- pandas
- scikit-learn

`pip install torch numpy pandas scikit-learn`

## Come eseguire
1. Estrarre il dataset in formato .csv dal file **train_test_network.rar**
2. Aprire il notebook **data_poisoning_test.ipynb**
3. Eseguire le due celle in ordine per lanciare il test coi parametri predefiniti
4. Eventualmente è possibile prima modificare i parametri nella sezione dedicata (Fase 2) per un test personalizzato:

                  porzione_classe_target = 0.1

                  poison_rate_value = 0.5

                  classe_da_avvelenare = 1

                  classe_destinazione = 0

                  rumore_std = 0.05

                  epoche_per_iterazione = 6

                  num_iterazioni = 1
