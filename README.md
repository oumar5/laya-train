# Laya pour le reranking de passages en français

Le [notebook d’entraînement](laya-train.ipynb) spécialise Laya pour le tri de passages et l’estimation de la possibilité de répondre dans un système RAG.

## Résultats sur `laya-eval`

| Classement | Hit@1 | Hit@7 | AUROC de pertinence |
| --- | ---: | ---: | ---: |
| Recherche seule | 54,44 % | 96,67 % | — |
| mMARCO multilingue | 67,78 % | 96,67 % | 0,8490 |
| Laya, époque 4 | 67,78 % | 97,78 % | 0,8584 |
| Laya, époque 7 | 64,44 % | 97,78 % | 0,8846 |

mMARCO et Laya à l’époque 4 placent un passage pertinent en tête pour 61 questions sur 90. Dans les sept premiers résultats, Laya en couvre 88 et mMARCO 87. Le checkpoint de l’époque 7 a la meilleure AUROC et place un passage pertinent en tête pour 58 questions.

## Protocole

- Les modèles utilisent les mêmes 1 573 paires question–passage, réparties sur 165 questions. Le Hit@1 et le Hit@7 sont calculés sur les 90 questions qui ont au moins un passage pertinent. Le calcul prend l’espérance du classement en cas d’égalité. L’AUROC porte sur les 1 573 paires.
- Le comparatif utilise le [checkpoint public mMARCO multilingue](https://huggingface.co/cross-encoder/mmarco-mMiniLMv2-L12-H384-v1/tree/1427fd652930e4ba29e8149678df786c240d8825) avec un maximum de 512 tokens par paire.
- La reprise de Laya s’est arrêtée à la fin de l’époque 9 sur un plafond de 20. Le meilleur checkpoint selon l’AUROC moyenne de validation est celui de l’époque 7 (0,8580).
- Une mesure sur le jeu indépendant `golden/` reste à faire pour confirmer le classement des rerankers sur le banc R-54.
