# Project02_CatastrophicForgetting
Alma Mater Studiorum - Università di Bologna, Corso di Cybersecurity M, anno 2025/2026, Michele Colajanni

Gruppo 20: Sergio Romeo, 0001092962
Progetto numero 2: Adversarial Attack using the Catastrophic Forgetting 

Questo progetto consiste nell'implementazione in PyTorch di un modello di **class-incremental learning** con pone l'obiettivo di studiare il **catastrophic forgetting** in presenza di un **data poisoning attack**, cercando di quantificare il numero di dati avvelenati necessario a ridurre le capacità del sistema.

## Caratteristiche principali

- Modello: **MLP**
- Approccio: **Class-Incremental Learning**
- Replay Buffer: dimensione configurabile (default 4000)
- Fase 1: Training sequenziale su 5 task (Priorità alle classi meno frequenti)
- Fase 2: Training con dati "avvelenati":
  - Perturbazione sulle feature numeriche
  - Label flipping mirato
- Metriche: Accuracy globale + classification report completo

## Dataset

- File: `train_test_network.csv`
- Classi (10): normal (0), backdoor (1), ddos (2), dos (3), injection (4), mitm (5), password (6), ransomware (7), scanning (8), xss (9)
- Preprocessing: one-hot su `proto` e `conn_state`, standard scaling su tutte le feature, sostituzione `-` → 0
- Split stratificato: 70% train / 30% test


########################################################################################## Requisiti
- Python 3.8+
- PyTorch (torch, torch.nn, torch.optim)
- numpy, pandas, scikit-learn

pip install torch numpy pandas scikit-learn

########################################################################################## Come eseguire
1. Posizionare il file train_test_network.csv nella stessa cartella
2. Aprire il notebook data_poisoning_test.ipynb
3. Eseguire le due celle, eventualmente modificando i parametri nella sezione dedicata (Fase 2):

-----------------
porzione_classe_target  = 0.30
poison_rate_value       = 0.70         # Percentuale di esempi da avvelenare
classe_da_avvelenare     = 1           # 0=normal, 1=backdoor, 5=mitm, etc.
classe_destinazione      = 0           # Label dopo flip
rumore_std              = 0.05
epoche_per_iterazione   = 6
num_iterazioni          = 3
-----------------
