# Работа в редакторе кода

Чтобы работать с кодом вашей функции в редакторе кода:
1. В [консоли управления]({{ link-console-main }}) выберите сервис **{{ sf-name }}**.
1. Нажмите значок ![image](../../../_assets/vertical-ellipsis.svg) в строке функции, которую вы хотите редактировать.
1. В открывшемся окне перейдите в раздел **Редактор**.
1. В блоке **Код функции**:
    - Выберите среду выполнения.
    - Выберите способ редактирования функции: **Редактор кода**.
    - В окне для редактирования функции выберите существующий файл в панели навигации слева или создайте новый, нажав кнопку **Создать файл**. В открывшемся окне введите имя файла с расширением и нажмите **Создать**.
    - Укажите точку входа — имя функции, которая будет вызываться в качестве обработчика. Указывается в формате `<имя файла с функцией>.<имя обработчика>`, например, `index.myFunction`.
1. В блоке **Параметры**:
    - Укажите таймаут.
    - Укажите объем памяти.
    - (опционально) Выберите или создайте [сервисный аккаунт](../../../iam/concepts/users/service-accounts.md). При добавлении сервисного аккаунта для функции, вы сможете [получать его IAM-токен из контекста функции](../function-sa). 
    - (опционально) Добавьте переменные окружения.
1. В верхнем правом углу нажмите **Создать версию**.      