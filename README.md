### Задача "Понимание JVM"

ОПИСАНИЕ СТРОК: 
1.  на СТЭКЕ-во-фрейм-main  - выделяется область памяти под переменную int i = 1
2.  новый класс Object и он отдается в систему загрузки Классов  ClassLoader'ы, где проходит 3-уровня, В Application ClassLoade* JVM находит класс Object  и происходит подгрузка мета-информации о данном классе в специальную область памяти Metaspace и  поле чего на этап связывания - проверка на валидность, разрешение ссылок на загруженный класс Object) , далее Object проходит этап Initialization. Создается объект класса Object в области памяти КУЧА, а ссылка о- на объект Object будет храниться на СТЭКЕ-во-ФРЭЙМ-main
3. Новый класс Integer (загружаем класс через -> ClassLoader'ы -> Связываеие -> Инициализация ->  загрузка в область памяти Metaspace ) выделяется память и создается экземпляр класса Integer, а ссылка ii - на Integer будет храниться на СТЭКЕ-во-ФРЭЙМ-main
4.  происходит вызов метода printAll(o, i, ii); и на СТЭКЕ создается ФРЕЙМ-printAll:  
1-Передаем ссылку на объект Object (хранится ссылка на СТЭКЕ-во-ФРЭЙМ-printAll)   
2-Создается новый экземпляр переменной int i(значение хранится  на СТЭКЕ-во-ФРЭЙМ-printAll)
3-Передаем ссылку на объект Integer (хранится ссылка на СТЭКЕ-во-ФРЭЙМ-printAll)
5. в КУЧЕ  выделяется память и создается экземпляр класса Integer, а ссылка uselessVar - на Integer будет храниться на СТЭКЕ-во-ФРЭЙМ-printAll
6. Новый класс System  (загружаем класс через -> ClassLoader'ы -> Связывание -> Инициализация ->  загрузка в область памяти Metaspace )
Cоздается новый ФРЕЙМ метода println. Этому методу передаем:
1-Передаем ссылку на объект Integer (хранится ссылка на СТЭКЕ-во-ФРЭЙМ-println)
2-Создается новый экземпляр переменной int i(значение хранится  на СТЭКЕ-во-ФРЭЙМ-println)
3-Вызывается метод класса Object через ссылку o.toString() и на СТЕКЕ создается новый ФРЕЙМ метода toString() объекта  Object.
Далее JVM выполняет метод toString(), после выполнения метода (ФРЕЙМ метода toString())-автоматически удаляется и выполнение переходит к методу ФРЭЙМ-println.
Метод  println() - выполняется.
После выполнения метода System.out.println(), ФРЭЙМ-println и все ссылки и экземпляры переменной- автоматически удаляются из СТЭКА и если будет вызван СБОРЩИК МУСОРА - то объект - Integer (ссылка uselessVar - была удалена со СТЭКА) и следовательно сам объект удаляется из КУЧИ.
7.  создается новый ФРЕЙМ метода System.out.println, методу передаем: константу "finished" и в КУЧЕ создается и хранится объект типа String со значением "finished".
Метод  println() - выполняется.
После выполнения метода System.out.println(), ФРЭЙМ-println и все ссылки и экземпляры переменной- автоматически удаляются из СТЭКА и если будет вызван СБОРЩИК МУСОРА - то объект - String (константа не имеет ссылки) и следовательно сам объект со значением-"finished" удаляется из КУЧИ.
Метод main() - выполняется и его ФРЕЙМ удаляется из СТЭКА и вся программа завершается, СБОРЩИК МУСОРА все удаляет из КУЧИ и область памяти Metaspace очищается.


При запуске кода класса JavaCoreUnderstandingJVM для выполнения программы сначала подгружает системные классы
(необходимые для выполнения программы) и сам класс JvmComprehension через подсистему загрузчиков классов: ClassLoader'ы
1. При поступлении запроса на подгрузку класса, Application ClassLoader перенаправляет запрос в
   Platform ClassLoader, который в свою очередь в Bootstrap ClassLoader
2. В Bootstrap ClassLoader - происходит выполнение запроса - поиск класса из состава JDK,
   класс JavaCoreUnderstandingJVM не найден, следовательно делегация поиска идет в обратном порядке,
3. В Platform ClassLoader идет поиск в сторонних библиотеках,
   класс JavaCoreUnderstandingJVM не найден, следовательно делегация поиска переходит в Application ClassLoader
4. В Application ClassLoader JVM находит класс  JavaCoreUnderstandingJVM и происходит подгрузка мета-информации о данном классе
   в специальную область памяти Metaspace (по дефолту объем не ограничен), где будет хранится информация о
   (имени класса JavaCoreUnderstandingJVM) методах:
    - main()
    - printAll();
    - о переменных
5. Далее JVM переходит на этап связывания, где на уровне Verify проходит подготовка класса JavaCoreUnderstandingJVM на его валидность и к выполнению,  
   далеe на этапе Prepare   - выделяется память под статические переменные, но в в классе JavaCoreUnderstandingJVM таковых нет,
   далее на этапе Resolve происходит связывание сылок надругие классы - на данном этапе их нет и JVM передает выполнение подсистеме Initialization
6. в Initialization выполняется  статические инициализаторы, в классе JavaCoreUnderstandingJVM их нет. Начинает выполнение метод main().