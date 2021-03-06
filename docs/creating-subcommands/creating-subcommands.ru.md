# Создание подкоманд

Начиная с версии`bem-tools` `1.0.0` cтандартный набор команд можно расширить при помощи модулей
подкоманд. Каждая покоманда представляет из себя npm пакет, который устанавливается в ту же папку,
что и `bem-tools`.

Чтобы `bem-tools` смог загрузить и выполнить внешнюю подкоманду имя пакета должно быть записано в
формате `bem-<команда>`. Таким образом, при попытке выполнить `bem foo` будет загружен модуль
`bem-foo`.

Для написания подкоманд используется библиотека [COA](https://github.com/veged/coa).
Модуль подкоманды должен экспортировать объект `coa.Cmd` или функцию, конфигурующую этот объект. В последнем
случае объект команды передается в функцию в качестве контекста (`this`). Пример:

```javascript
var Q = require('q');
module.exports = function() {
    return this.title('hello world command')
        .act(function() {
            return Q.resolve('Hello, world!');
        });
};
```

За дополнительными примерами и справочной информацией по API COA обращайтесь
к [документации этой библиотеки](https://github.com/veged/coa/blob/master/README.md).

## Использование заготовки tpl-bem-command

Для упрощения процесса создания подкоманд можно использовать шаблонизатор проекта [tpl-bem-command](https://github.com/bem/tpl-bem-command).

Чтобы воспользоваться шаблонизатором нужно:
* убедиться что в системе установлен [volo](http://volojs.org/):

    npm install -g volo

* создать новый проект из шаблона при помощи команды:

    volo create <папка проекта> bem/tpl-bem-command

В процессе работы шаблонизатора необходимо будет указать:

* имя команды;
* имя и email автора;
* репозиторий проекта на github.

В результате будет создана заготовка проекта для npm-пакета `bem-<имя команды>` со следующим содержимым:

* `package.json` c уже подключенными библиотеками [q](https://github.com/kriskowal/q),
    [COA](https://github.com/veged/coa) и [update-notifier](https://github.com/yeoman/update-notifier/);
* файл `lib/<имя команды>.js`, в котором будет содержаться код подкоманды;
* файл `README.md` с инструкцией по подключению подкоманды к bem-tools;
* файл `bin/bem-<имя команды>`, который может использоваться для запуска команды отдельно от `bem-tools`;
