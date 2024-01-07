# esd-ddp
[Efficient SciDev. Homework -- DDP](https://drive.google.com/drive/folders/1LWLkut23ovI0oza4bYHRLvGuqnMrz5WJ)

Cократила обучающий датасет в два раза.
Провела эксперименты перебирала:
* модель
* c ddp и без
* ddp_n_proc - 2/4
* batch_size - 64/128/256
Обучала все на 15 эпохах, результаты в таблички по каждой 5

В **base_utils.py** лежат функции для экспериментов без ddp

B **ddp_utils.py** лежат функции для экспериментов с ddp

B **lightning_utils.py** перевод в lightning

| model         | ddp           | batch_size    | epoch          | ddp_n_proc    | accuracy      |
| ------------- | ------------- | ------------- | -------------- | ------------- | ------------- |
| Toy           | -             |64             |5               |-              |0.9173         |
| Toy           | -             |64             |10              |-              |0.925          |
| Toy           | -             |64             |15              |-              |0.9258         |
| Toy           | -             |128            |5               |-              |0.9095         |
| Toy           | -             |128            |10              |-              |0.9186         |
| Toy           | -             |128            |15              |-              |0.9222         |
| Toy           | -             |256            |5               |-              |0.9086         |
| Toy           | -             |256            |10              |-              |0.9201         |
| Toy           | -             |256            |15              |-              |0.9242         |
| Toy           | +             |64             |5               |2              |0.9135         |
| Toy           | +             |64             |10              |2              |0.9217         |
| Toy           | +             |64             |15              |2              |0.9253         |
| Toy           | +             |128            |5               |2              |0.9007         |
| Toy           | +             |128            |10              |2              |0.9166         |
| Toy           | +             |128            |15              |2              |0.9218         |
| Toy           | +             |256            |5               |2              |0.8809         |
| Toy           | +             |256            |10              |2              |0.9059         |
| Toy           | +             |256            |15              |2              |0.9165         |
| Toy           | +             |64             |5               |4              |0.9007         |
| Toy           | +             |64             |10              |4              |0.9166         |
| Toy           | +             |64             |15              |4              |0.9218         |
| Toy           | +             |128            |5               |4              |0.8809         |
| Toy           | +             |128            |10              |4              |0.9059         |
| Toy           | +             |128            |15              |4              |0.9165         |
| Toy           | +             |256            |5               |4              |0.8596         |
| Toy           | +             |256            |10              |4              |0.8943         |
| Toy           | +             |256            |15              |4              |0.9065         |
| MobileNetv3   | -             |64             |5               |-              |0.9238         |
| MobileNetv3   | -             |64             |10              |-              |0.961          |
| MobileNetv3   | -             |64             |15              |-              |0.9658         |
| MobileNetv3   | -             |128            |5               |-              |0.9297         |
| MobileNetv3   | -             |128            |10              |-              |0.9464         |
| MobileNetv3   | -             |128            |15              |-              |0.9521         |
| MobileNetv3   | -             |256            |5               |-              |0.882          |
| MobileNetv3   | -             |256            |10              |-              |0.9144         |
| MobileNetv3   | -             |256            |15              |-              |0.9288         |
| MobileNetv3   | +             |64             |5               |2              |0.9373         |
| MobileNetv3   | +             |64             |10              |2              |0.9523         |
| MobileNetv3   | +             |64             |15              |2              |0.963          |
| MobileNetv3   | +             |128            |5               |2              |0.9189         |
| MobileNetv3   | +             |128            |10              |2              |0.9431         |
| MobileNetv3   | +             |128            |15              |2              |0.9539         |
| MobileNetv3   | +             |256            |5               |2              |0.8765         |
| MobileNetv3   | +             |256            |10              |2              |0.9159         |
| MobileNetv3   | +             |256            |15              |2              |0.9291         |
| MobileNetv3   | +             |64             |5               |4              |0.9226         |
| MobileNetv3   | +             |64             |10              |4              |0.948          |
| MobileNetv3   | +             |64             |15              |4              |0.9531         |
| MobileNetv3   | +             |128            |5               |4              |0.8894         |
| MobileNetv3   | +             |128            |10              |4              |0.923          |
| MobileNetv3   | +             |128            |15              |4              |0.9365         |
| MobileNetv3   | +             |256            |5               |4              |0.8343         |
| MobileNetv3   | +             |256            |10              |4              |0.9016         |
| MobileNetv3   | +             |256            |15              |4              |0.9158         |

Теперь сравнение по времени(тут оно усредненное, в секундах):

| model         | ddp           | batch_size    | ddp_n_proc     | t_batch       |t_epoch        |
| ------------- | ------------- | ------------- | -------------- | ------------- | ------------- |
| Toy           | -             |64             |-               |0.0003269043   |0.15331812     |
| Toy           | -             |128            |-               |0.0003937329   |0.09252723     |
| Toy           | -             |256            |-               |0.000501416    |0.059167083    |
| Toy           | +             |64             |2               |0.0015535179   |0.3650767      |
| Toy           | +             |128            |2               |0.0016661187   |0.19660202     |
| Toy           | +             |256            |2               |0.002174578    |0.1283001      |
| Toy           | +             |64             |4               |0.0040323758   |0.47582036     |
| Toy           | +             |128            |4               |0.0049461797   |0.2918246      |
| Toy           | +             |256            |4               |0.0066325553   |0.19897667     |
| MobileNetv3   | -             |64             |-               |0.9244106      |433.5485       |
| MobileNetv3   | -             |128            |-               |1.2256594      |288.02994      |
| MobileNetv3   | -             |256            |-               |1.5930047      |187.97458      |
| MobileNetv3   | +             |64             |2               |1.4124687      |331.93015      |
| MobileNetv3   | +             |128            |2               |1.8201376      |214.77623      |
| MobileNetv3   | +             |256            |2               |2.1577322      |127.30622      |
| MobileNetv3   | +             |64             |4               |2.195149       |259.02762      |
| MobileNetv3   | +             |128            |4               |2.654641       |156.62381      |
| MobileNetv3   | +             |256            |4               |2.654641       |95.10962       |

Графики зависимости времени батча от количества процессов:

<img width="590" alt="Screenshot 2024-01-07 at 22 44 55" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/8e8b3e28-c84e-48b6-bd13-8376c60a5aa0">
<img width="590" alt="Screenshot 2024-01-07 at 22 48 54" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/e4e853d8-4a8d-4c8f-8af5-eb4b0a5216bc">

Графики зависимости времени эпохи от количества процессов:

<img width="590" alt="Screenshot 2024-01-07 at 22 45 21" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/e20b428e-b75c-4907-ac7f-a3429c46b8a9">
<img width="590" alt="Screenshot 2024-01-07 at 22 49 14" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/07b556d7-b560-406b-98c1-6eec83201369">


Графики зависимости времени батча от размера бачта:

<img width="590" alt="Screenshot 2024-01-07 at 22 57 06" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/14bb3d9b-5209-4b06-bbd2-28a3ce93b376">
<img width="590" alt="Screenshot 2024-01-07 at 22 36 49" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/877892fd-a0cd-4991-9d56-ef35ef2a20a5">

Графики зависимости времени эпохи от размера батча:

<img width="590" alt="Screenshot 2024-01-07 at 22 57 34" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/680ba4f9-09b0-4737-99f4-c6971c886f86">
<img width="590" alt="Screenshot 2024-01-07 at 22 36 04" src="https://github.com/a4-ai/esd23-distrib-av-onishchenko/assets/79263390/71b1f89f-c6ad-4a48-9efc-79fcc9cb4105">

