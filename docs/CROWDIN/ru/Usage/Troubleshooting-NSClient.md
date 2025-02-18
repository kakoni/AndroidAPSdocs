# Устранение неисправностей клиента Nightscout

Для правильной работы NSClient требуется стабильная коммуникация с сайтом Nightscout. Нестабильная связь приводит к ошибками синхронизации и высокой интенсивности использования данных.

Если никто не следит за вами на Nightscout вы можете приостановить NSClient для экономии заряда аккумулятора или вы можете настроить NSClient таким образом, чтобы он подключался только по Wi-Fi и/или во время зарядки.

* Как обнаружить нестабильную связь?

Перейдите на вкладку NSClient в AAPS и просмотрите журнал событий. Обычно отклик PING происходит каждые ~ 30 секунд и сообщения о повторном подключении не поступают. Если вы видите много повторных попыток соединения, то это свидетельство проблемы связи.

Начиная с версии AndroidAPS 2.0, при обнаружении такого поведения, происходит приостановка NSClient на 15 минут и на главном экране появляется сообщение "Сбой (ошибка) NSClient".

* перезапуск

В качестве первого шага попробуйте перезапустить Nightcout и затем телефон, чтобы понять, сохраняется ли проблема.

Если Nightscout размещен на Heroku, вы можете перезапустить Nightscout так: зайдите в Heroku, нажмите на имя приложения, нажмите в меню «More », затем «Restart all dynos».

На других хостингах следуйте документации хостинга.

* Проблемы с телефоном

Android может перевести телефон в спящий режим. Убедитесь, что AndroidAPS в опциях питания имеет разрешение на постоянную работу в фоновом режиме.

Проверьте NSClient заново в надежном месте сетевого сигнала.

Попробуйте другой телефон.

* Nightscout

Если ваш сайт размещен на Azure: Многие люди обнаружили, что проблемы подключения уменьшились после перехода на Heroku.

Для решения проблем подключения в Azure необходимо ВКЛЮЧИТЬ в настройках приложения HTTP протокол 2.0 и Websockets

* Если все еще приходят сообщения об ошибке...

Проверьте размер вашей базы данных в MongoDB (или через плагин размера базы данных в NS). Если вы используете бесплатный платежный план в MongoDB, то 496MB означает, что база заполнена и ее следует очистить. [Следуйте этим инструкциям Nightscout для проверки размера вашей базы данных и удаления данных](https://nightscout.github.io/troubleshoot/troublehoot/#database-full).

Перед очисткой данных из базы данных и подумайте о передаче своих данных AndroidAPS в проект Open Humans (для исследований). Инструкции находятся на [странице конфигурации OpenHumans](../Configuration/OpenHumans).