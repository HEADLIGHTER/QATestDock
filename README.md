# Полное тестирование функциональности
Ввод текста должен осуществляться на различных раскладках, как минимум двух (RU) (EN).
## Оглавление
* [Поля карточки поручения](#0)
* [Процесс по состояниям поручения](#1)
* [Отзыв поручения](#2)
  <a name="headers"/>

### <a name=0>0</a> : Поля карточки поручения

  0. Проверить автоматически заполненные и не доступные для редактирования реквизиты на соответствие требованиям:
     * **Номер** - по порядку 1 2 3...
     * **Основание** - ссылка на документ, из которого создана карточка Поручения
     * **Родительское** поручение - ссылка на поручение, для корневого поручения пусто
     * **Дата создания** - текущая дата и время
     * **Состояние** и **Состояние изменено** – состояние по процессу, меняется автоматически
     * **Автор** - сотрудник, автозаполняется текущим
     * **Фактическая дата исполнения** - дата без времени, автозаполняется при исполнении поручения

  1. Попытаться изменить все недоступные для редактирования реквизиты карточки путем ввода, удаления и вставки текста, 
     в том числе спецсимволов.

  2. Проверить возможность вставки, ввода спецсимволов и текста, отличного от формата данных сотрудников для следующих реквизитов:
     * **Контролер** 
     * **Ответственный исполнитель** - проверить невозможность отправить на исполнения введя неверного пользователя или
     не указав данный реквизит.
     * **Соисполнители** - Дополнительно: проверить невозможность назначить **ответственного исполнителя** в соисполнители
     
     Так же попытаться указать несуществующего сотрудника в каждом из этих полей.
     
  3. Проверить возможность ввода некорректных значений в поле **Срок исполнения**, в том числе отрицательные, прошедшие, 
     краевые и превышающие их даты. Попытаться указать время.

  4. Проверить возможность ввода, удаления и вставки текста, в том числе спецсимволов для поля **Текст поручения**.

  5. **Ссылки**: 
     * Проверить возможность ввода, удаления и вставки текста, в том числе спецсимволов
     * Если есть такой функционал, проверить возможность ввода некорректных ссылок (htt;\/wvw.ex`ample.ru)
     * Возможность ввести несколько ссылок
     
  6. **Файлы**:
     * Возможность загрузить один и несколько файлов
     * Узнать максимальный объем одного и нескольких загружаемых файлов
     * Узнать разрешенные и запрещенные форматы файлов
     * Проверить возможность загрузки запрещенного типа файла с форматом разрешенного и наоборот
     * Дополнительно проверить GIF файлы если имеется поддержка данного формата
     * Проверить возможность загрузки вредоносных файлов
  
  7. Для всех текстовых полей произвести следующие операции:
     * Ввести самое большое или длинное отрицательное и положительное число 
     * Узнать максимальное и минимальное количество символов в каждом поле
     * Ввод всех доступных спецсимволов в одно поле
     * Протестировать перенос строки в однострочных полях
     * SQL инъекция (DROP TABLE users; если тестирование проходит на тестовом стенде, SELECT * FROM users; на Live)
     * XSS с произвольным кодом (<script>alert("1337")</script>)
     * Ввод полностью корректных значений
     * Удостовериться в правильности работы кнопок автозаполнения [...], в том числе календаря и файлов
     
  8. Проверить невозможность отправки на исполнение при наличии незаполненных или некорректно заполненных обязательных и необязательных
     полей, включая проверку по каждому отдельному полю и с использованием "пустых" вводов с помощью пробелов и спецсимволов.

### <a name=1>1</a> : Процесс по состояниям поручения

  0.  Проверить соответствие реквизитов и состояния поручения после отправки на исполнение, в том числе задания отправленные
      ответственному исполнителю и соисполнителям. Скачать все прикрепленные файлы и удостовериться в их работоспособности. 

  1. В случае если форма является частью функционала сайта, после отправки поручения вернуться на предыдущую страницу и
     попытаться создать новое поручение с теми же реквизитами.

  2. Попытаться изменить поля в созданном поручении.

  3. При отправке на контроль произвести следующие действия с полем ввода комментария:
     * Оставить пустым
     * Проверить ввод, вставку и удаление текста и спецсимволов
     * "Пустой" ввод из пробелов или спецсимволов
     * Протестировать перенос строки
     * Узнать максимальное и минимальное количество символов
     * Ввести самое большое или длинное отрицательное и положительное число
     * Ввод всех доступных спецсимволов в одно поле
     * SQL инъекция (DROP TABLE users; если тестирование проходит на тестовом стенде, SELECT * FROM users; в ином случае)
     * XSS с произвольным кодом (<script>alert("1337")</script>)
     
  4. В случае отправки на контроль соисполнителем, удостовериться что задание отправляется именно ответственному исполнителю.
     Проверить отзыв всех незавершенные дочерних поручений текущего исполнителя\задания и их дочерних и так далее.

  5. В случае отправки на доработку убедиться в повторном появлении задачи с комментарием ответственного исполнителя.
     Аналогично для принятия задачи и ее перехода в статус завершенного.

  6. При отправке на контроль ответственным исполнителем убедиться в изменении состояния задачи, и корректности 
     отправки контролеру или автору. Так же проверить отзыв по всему дереву незавершенных, дочерних и дочерних дочерним поручений,
     в том числе поручений ответственного исполнителя. Сверить фактическую дату исполнения с текущей.

  7. Повторить все шаги для дочернего поручения. Так же убедиться в том что оно назначается дочерним. 

  8. Для контролера:
     * Проверить корректность состояния задачи при принятии, убедиться в уведомлении об исполнении автора задачи
     * При отправке на доработку убедиться в корректном изменении статуса и наличии комментария контролера.
     
### <a name=2>2</a> : Отзыв поручения

  0. Убедиться в том что отзыв поручения доступен только автору

  1. Удостовериться в правильности работы окна подтверждения

  2. Убедиться в отзыве всех заданий и дочерних поручений по цепочке