## Configuration
- **Architecture**: MobileNet-v2
- **Epochs**: 30
- **Fragments**: 9 central parts
- **Batch Size**: 16
- **Number of Classes**: 5
- **Image Preprocessing**: Нормализация без обрезки пикселей, новая функция для деления на фрагменты

## Augmentations
We have utilized several augmentation techniques to improve the model's performance and robustness:
1. **Horizontal Flip**: Probability of 0.3
2. **Rotation**: Limited to 30 degrees with a probability of 0.3
3. **Salt and Pepper**: Applied only to the pepper with a ratio of 0.0
4. **Elastic Transform**: With parameters alpha=2, sigma=20, alpha_affine=10, and a probability of 0.4
  ```python
  HorizontalFlip(p=.3)
  Rotate(limit=30, p=.3)
  SaltAndPepper(salt_ratio=1.)
  ```

## Training Setup
- **Optimizer**: AdamW
  ```python
  torch.optim.AdamW(mobilenet_v2_model.parameters())

- **Loss Function**: CrossEntropyLoss with class weights. Class Weights: [0.1845, 0.1877, 0.1960, 0.2145, 0.2656]
  ```python
  nn.CrossEntropyLoss(weight=class_weights.to(device))

- **Schedular**: 
  ```python
  milestones = [12, 15, 26]
  gamma = 0.3
  torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones, gamma=gamma)

## Results

### Точность на объединенных тренировочных и валидационных данных
![Train Valid Accuracy](images/train_valid_Acc.jpg)

### Потери на объединенных тренировочных и валидационных данных
![Train Valid Loss](images/train_valid_Loss.jpg)

### Точность на тренировочных данных
![Train Accuracy](images/train_Acc.jpg)

### Потери на тренировочных данных
![Train Loss](images/train_Loss.jpg)

### Точность на валидационных данных
![Validation Accuracy](images/valid_Acc.jpg)

### Потери на валидационных данных
![Validation Loss](images/valid_Loss.jpg)

## Notes:
С нормализацией, без обрезания пикселей и без фильтров, с новым scheduler
Изменил только количество пикселей для соли и перца
Аугментация - соль и перец
Качество на полных изображениях - 65.306