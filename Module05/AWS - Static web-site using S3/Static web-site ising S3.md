# Создаем статичный веб-сайт на Amazon S3

## Введение
В этой лабораторной работе мы с вами создадим простой статичный веб-сайт и зададим базовые настройки его конфигурации. Этот пример продемонстрирует,  как легко можно создать хостинг для веб-сайта, который будет содержать файлы: HTML, CSS, JavaScript, шрифты и изображения.

## Решение
Авторизуйтесь на сайте https://aws.amazon.com/free/.
> Код нашего веб-сайта находится [здесь](https://github.com/LakshinEdgar/dl).

### Создаем S3 Bucket 
1. Находим сервис _S3_ в главном меню и открываем его.
![First image](./images/1.png)
![](https://github.com/LakshinEdgar/dl/blob/main/images/2.png)
2. Кликаем на кнопку **Create bucket**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/3.png)
3. Устанавливаем значения:

_Bucket name_: **mywebsitebuckey**<ID вашего аккаунта>  
_Region_: **EU (Stockholm) eu-north-1**

Значение _Bucket name_ должно быть уникальное, поэтому можно сделать его составным из произвольного названия и ID вашего аккаунта (замените <ID вашего аккаунта> вашим значением).  
Значение _Region_ для данной лабораторной работы не так важно, поэтому можно оставить значение по умолчанию.

![](https://github.com/LakshinEdgar/dl/blob/main/images/5.png)
4. В настрйоках _Bucket settings for Block Public Access_ снимаем галочку _Block all public access_.
Необходимо убедиться, что также сняты галочки со всех 4-х ограничений разрешений под ним.

5. Убеждаемся, что стоит галочка в _Turning off all public access might result in the bucket and its objects becoming public_.
![](https://github.com/LakshinEdgar/dl/blob/main/images/6.png)

6. Остальные настройки оставляем по умолчанию.

7. Кликаем  **Create bucket**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/7.png)

8. Кликаем на созданный нами __Bucket__.
![](https://github.com/LakshinEdgar/dl/blob/main/images/8.png)

9. Кликаем на кнопку **Upload**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/9.png)

10. Кликаем на кнопку **Add files** и загружаем 2 html файла из локального компьютера error.html и index.html.
Скачать их можно на локальный компьютер [отсюда](https://github.com/LakshinEdgar/dl)
![](https://github.com/LakshinEdgar/dl/blob/main/images/10.png)
![](https://github.com/LakshinEdgar/dl/blob/main/images/11.png)

11. Остальные настройки оставляем по умолчанию.
12. Кликаем на кнопку **Upload**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/12.png)

13. Кликаем на кнопку **Close**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/14.png)


### Включаем Static Website Hosting
1. Переходим на вкладку _Properties_.
![](https://github.com/LakshinEdgar/dl/blob/main/images/16.png)

2. Скроллим до самой нижней части страницы и находим блок _Static website hosting_.
![](https://github.com/LakshinEdgar/dl/blob/main/images/17.png)

3. Кликаем на копку **Edit**.

4. Устанавливаем следующие значения:  
_Static website hosting_: **Enable**  
_Hosting type_: **Host a static website**  
_Index document_: **index.html**  
_Error document_: **error.html**  
![](https://github.com/LakshinEdgar/dl/blob/main/images/18.png)

5. Кликаем на кнопку **Save changes**.
6. Скроллим до блока _Static website hosting section_. Переходим по ссылке.
![](https://github.com/LakshinEdgar/dl/blob/main/images/19.png)

7. Должна открыться на новой вкладке браузера страница с 403 ошибкой (ошибка доступа).
![](https://github.com/LakshinEdgar/dl/blob/main/images/20.png)

### Настраиваем политику доступа к нашему Bucket
1. Вернитесь в сервис _S3_ и перейдите на вкладку _Permissions_.
![](https://github.com/LakshinEdgar/dl/blob/main/images/21.png)

2. В блоке _Bucket policy_ нажмите на кнопку **Edit**.
![](https://github.com/LakshinEdgar/dl/blob/main/images/22.png)

3. В блоке _Policy_ вставляем кусок JSON-файла ниже. <BUCKET_ARN> необходимо заменить на ваш ARN. Он указан выше поля для ввода.
Убедитесь, что символы /* присутствуют в коде, который вы только что вставили, т.к. именно эти символы открывают доступ ко всем объектам в _Bucket_.
```json
{
 "Version":"2012-10-17",
 "Statement":[{ 
     "Sid":"PublicReadGetObject", 
     "Effect":"Allow", 
     "Principal": "*", 
     "Action":["s3:GetObject"], 
     "Resource":["<BUCKET_ARN>/*"] 
     }] 
}
```
![](https://github.com/LakshinEdgar/dl/blob/main/images/23.png)

4. Кликните на кнопку **Save changes**.
5. Обновите страницу браузера, на которой была ошибка 403. После перезагрузки страницы должно все работать корректно.
![](https://github.com/LakshinEdgar/dl/blob/main/images/24.png)
6. Попробуйте добавить / и любой символ после него к адресу нашей страницы и нажать кнопку Enter. В результате должна загрузиться страница error.html.
![](https://github.com/LakshinEdgar/dl/blob/main/images/25.png)
![](https://github.com/LakshinEdgar/dl/blob/main/images/26.png)

