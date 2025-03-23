# Compression project | Пособие по сжатию форматов

The project is focused on learning and is 
designed to analyze in detail the methods of
compressing images, audio and videos using illustrated step-by-step
examples on Python

Этот проект нацелен на обучение и разработан для детального
анализа методов сжатия картинок, аудио и видео, с использованием
иллюстрированных пошаговых примеров на Python.


## *Изображения | Images*
### jpeg 

Шаги:
1. перевод из формата RGB в формат YCrCb (обратное преобразование требуется на последующих этапах)
   
   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/rgb2ycrcb.JPG)
   
2. понижение размерности каналов Cr and Cb в два раза, не влияет на восприятие картинки человеком,
   но сразу даёт понижение размера вдвое: (1+1+1) = 2*(1+1/4+1/4)
   
   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/downsampling.JPG)
   
3. примение дискретного косинусного преобразования (ДКП) к одиночным каналам. Для этого изображение делится на квадраты 8х8.
   Применение дкп - это приведение к базису особо вида, разложение "функции" на составляющие из гармоничесих функций. При этом
   в левом верхнем углу значения изменяются с менее высокими частотами, чем в правом нижнем. Человек более восприимчив к плавным и крупным
   цветовым изменениям, поэтому мы можем безболезненно отбросить некоторую часть высокочастотных значений.
   Приведенное изображение соответсвует базису ДКП-II, разложение по нему можно воспринимать равнозначно утверждению,
   что любое одноканальное изображение можно составить из взвешенной комбинации этих изображений.
   
   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/basis.JPG)

4. деление полученной матрицы 8х8 на специальную матрицу коэффициентов, таким образом получаем
зануление части высокочастотных коэффициентов ДКП и, следовательно, снижение количества храншимой
информации

   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/DCT_classic.JPG)

5. обход "змейкой", при котором сначала выбираются более низкочастотные значения
   
   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/snake.JPG)
   
6. Сравнение различных степеней сжатия при обходе змейкой (не является частью алгоритма)
    
   ![](https://github.com/iiifd2u/Compression/blob/jpeg/jpeg/records/compressed.JPG)

7. Вытягивание матрицы 8х8 в вектор 64

8. Кодирование этого вектора алгоритмом Хаффмана с особыми условиями


Steps (needed to redact):
1. convert image from RGB to YCrCb color space
2. downscale Cr and Cb channel 
3. apply discrete cosine transform (DCT) to single channels, so we obtain distribution not by colors, but by frequencies. 
4. devide this matrix 8x8 to special jpeg-matrix, so as result we make zero many of high-freqs values in DCT matrix and great space economy.
5. flatten values of result matrix by zig-zag or snake path.
6. sum frequencies of all this flattened matricies and obtain distribution of frequencies of each value (0 to 256 of FF in hex16)
7. Apply Huffman algorithm to creat optimal coding table for this data.
8. Make Huffman codes canonical to economy much more space.
