### Структура проекта

Немного поговорим о том, как не надо программировать. Типичная ошибка это складывать весь код в один файл, как мы сделали в предыдущем уроке. Правильное распределение исходного кода по файлам упрощает понимание кода и работу с ним. К сожалению в C++ нет общепринятого метода распределения кода по отдельным файлам и директориям проекта, хотя есть некоторые рекомендации. Чаще всего лучше делать так, как принято в вашей команде, или же так, как принято в фреймворке с которым вы работаете (это позволяет унифицировать проект и облегчает обмен проектами).

Так что мы возьмем код из предыдущего проекта, и распределим его по отдельным файлам, исходя из рекомендаций, приведенных ниже. Отмечу, что это именно рекомендации по внешнему виду, они не влияют на корректность программы, но обеспечивают комфортную работу.

- В c++ файлы с кодом разделяются на заголовочные файлы (header, ), содержащие определения классов и функций, и собственно файлы с исходным кодом (source, сc-файл), содержащие реализацию функций и методов класса.
- Нет общепринятого стандарта на расширения файлов. Наиболее распространнённые расширения для заголовочных файлов это `.hh`, `.hpp`, `.hp`, `.h++`, `.hxx`,`.icc`,`.inl`,`.tcc`, для файлов с исходниками `.cc`,`.cpp`,`.cp`,`.cxx`,`.i`,`.ii`,`.m`,`.mm`. В GEANT4 используется `.hh` и `.cc`, этой нотации я и буду следовать, также в дальнейшем для краткости я буду употреблять слова hh-файл и сc-файл для соответствующего типа файла.
- Также нет общепринятого стандарта на расположение файлов в директории (в отличии от, например, Java и Python, где есть определенные требования к структуре проекта). В GEANT4 принято hh-файлы хранить в директории `include`, а cc-файлы (кроме файла с функцией `main`) в директории `src`. В качестве примера приведу структуру проекта после того, как мы корректно оформим программу из предыдущего урока:
```
.
├── include
│   ├── ActionInitialization.hh
│   ├── DetectorConstruction.hh
│   └── PrimaryGeneratorAction.hh
├── src
│   ├── ActionInitialization.cc
│   ├── DetectorConstruction.cc
│   └── PrimaryGeneratorAction.cc
├── CMakeLists.txt
├── init_vis.mac
└── main.cc
```
- Для каждого класса нужно писать отдельную пару hh- и cc-файлов. В заголовочных файлах желательно писать только определение класса, без реализации методов (что практически никем не соблюдается). Название файлов до расширения обычно делают совпадающим с именем класса. Для имен классов в GEANT4 принят PascalCase, то есть имя класса состоит из слов без разделителей, каждое слово начинается с заглавной буквы, например `PrimaryGeneratorAction`. 
- Прочие файлы распределяются удобным для вас образом, я обычно выделяю отдельные директории для gdml-файлов, макросов с описанием источников, управляющих макросов.

В следующем параграфе мы обсудим как организовать файл `CMakeLists.txt`, что бы он собирал наш код в работающую программу.
