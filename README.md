# Готовый компонент (Completed RXDTDeploy Component)

#### Описание:

Готовый к использованию компонент по сборке/доставке разработки.

Вызывает другие компоненты для различных задач: [ручной старт](https://github.com/STARKOV-Group/rx-startmode-component), [подготовка переменных](https://github.com/STARKOV-Group/RX-CheckVariables-Component), [подготовка сервера](https://github.com/STARKOV-Group/RX-PreparedServer-Component), [сборка пакета](https://github.com/STARKOV-Group/RX-BuildPackage-Component), [публикация пакета на сервер](https://github.com/STARKOV-Group/RX-DeployDTPackage-Component) и [выгрузка пакета в корпоративное облако](https://github.com/STARKOV-Group/RX-UploadPackage-Component).

#### Общие переменные:

##### **<span style="color: rgb(53, 152, 219);">Пользовательские настройки</span>**

##### RXVERSION

**Описание:** Версия RX (название папок в которых лежат DDS и DT для нужной версии RX)  
**Обязательность:** Да  
**Пример:** 25.2.0.39

##### DeployDestination

**Описание:** Префикс для переменных DeployIpServer, DeployUserName, DeployUserPassword  
**Примечание**: Лучше указывать в переменных этапа, для разделения данных для разных контуров  
**Обязательность:** Нет  
**Значение по умолчанию:** ""

##### NeedApplySettings

**Описание:** Флаг для обозначения необходимости применить настройки на сервер после публикации  
**Обязательность:** Нет  
**Возможные значения:** "True", "False"  
**Значение по умолчанию:** "False"

##### RunnerTags
**Описание:** Переменная для прокидывания тегов для определения используемого раннера в компоненты  
**Примечание:** Так под капотом это значение напрямую присваивается тегам в этапе, то пройдет перезапись любого дефолтного значения, указанного например с помощью default: tags в пайплайне проекта. Для общего тега указывать значение в пайплайне, иначе в каждом конкретной ветке. Если не важно, какой раннер будет использован, то можно вообще не указывать.  
**Обязательность:** Нет  
**Возможные значения:** Любое название тега ранера  

> [!note]
>Далее указаны сетевые настройки, их лучше не указывать напрямую, а создать переменные в CICD проекта.

##### WebProtocol

**Описание:** Протокол, по которому будет производится соединение к машине, на которой будет происходить публикация  
**Обязательность:** Да  
**Пример:** https

##### ServerHttpsPort

**Описание:** Порт для подключения по https к машине, на которой будет происходить публикация  
**Обязательность:** Да  
**Значение по умолчанию:** 443

##### ServerHttpPort

**Описание:** Порт для подключения по http к машине, на которой будет происходить публикация  
**Обязательность:** Да  
**Значение по умолчанию:** 80

##### BuildMode

**Описание:** Способ сборки пакета  
**Обязательность:** Да  
**Значение по умолчанию:** DebugRelease  
**Возможные варианты:** Release, DebugRelease, Source, SourceBase, OnlySourceBase

#### Переменные этапов:

##### <span style="color: rgb(224, 62, 45);">**Админские настройки**</span>

##### DDSFolderPath

**Описание:** Путь к папке со всеми DDS  
**Обязательность:** Да  
**Значение по умолчанию:** C:\\CICD\\DDS

##### DTFolderPath

**Описание:** Путь к папке со всеми DT  
**Обязательность:** Да  
**Значение по умолчанию:** C:\\CICD\\DTCore

##### AutoGenPackageUtilPath

**Описание:** Путь к папке с утилитой для генерации xml для сборки пакета  
**Обязательность:** Да  
**Значение по умолчанию:** C:\\CICD\\Tools\\PackageXMLGenerator

##### ProjectsFolderPath

**Описание:** Путь к папке с проектами на сервере  
**Обязательность:** Да  
**Значение по умолчанию:** C:\\CICD\\Projects

##### GitProjectFolder  

**Описание:** Путь к папке проекта  
**Обязательность:** Да  
**Значение по умолчанию**: \$ProjectsFolderPath\\[\$CI\_PROJECT\_NAMESPACE](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=project%2D1.-,CI_PROJECT_NAMESPACE,-Pre%2Dpipeline)\\[\$CI\_PROJECT\_NAME](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=the%20GitLab%20instance.-,CI_PROJECT_NAME,-Pre%2Dpipeline)  
**Примечание**: $CI\_PROJECT\_NAMESPACE и $CI\_PROJECT\_NAME - это параметры CICD GitLab'а по умолчанию, весь список можно посмотреть в [документации](https://docs.gitlab.com/ci/variables/predefined_variables/)  
**Пример:** C:\\CICD\\Projects\\SG

##### DDSConfigPath

**Описание:** Путь к папке с конфигурационным файлом DDS в папке группы проекта  
**Обязательность**: Да  
**Значение по умолчанию**: \$ProjectsFolderPath\\[\$CI\_PROJECT\_NAMESPACE](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=project%2D1.-,CI_PROJECT_NAMESPACE,-Pre%2Dpipeline)\\DDS\\$RXVERSION\\bin  
**Пример:** C:\\CICD\\Projects\\Alrosa\\DDS\\25.2.0.39

##### DTConfigPath

**Описание:** Путь к папке с с конфигурационным файлом DT в папке группы проекта  
**Обязательность**: Да  
**Значение по умолчанию**: $ProjectsFolderPath\\[\$CI\_PROJECT\_NAMESPACE](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=project%2D1.-,CI_PROJECT_NAMESPACE,-Pre%2Dpipeline)\\DT\\$RXVERSION  
**Пример:** C:\\CICD\\Projects\\Alrosa\\DT\\25.2.0.39

##### LogsPath

**Описание:** Путь к папке с логами раннера (лучше поставить как в примере, иначе не будут подтягиваться артефакты)  
**Обязательность**: Да  
**Значение по умолчанию:** [\$CI\_PROJECT\_DIR](https://docs.gitlab.com/ci/variables/predefined_variables/#:~:text=in%20GitLab%2017.8.-,CI_PROJECT_DIR,-Job%2Donly)

##### PackageProjectPath

**Описание:** Путь к папке для выгрузки собранного пакета (лучше использовать переменную гита $CI\_PROJECT\_DIR)  
**Обязательность:** Да  
**Значение по умолчанию:** $CI\_PROJECT\_DIR

##### GIT\_STRATEGY

**Описание**: Настройка способа подтягивания веток, подробнее в [документации](https://docs.gitlab.com/ci/runners/configure_runners/#git-strategy)  
**Обязательность**: Да  
**Значение по умолчанию:** none

> [!note]
>Далее идут совершенно не обязательные настройки названий этапов. Для корректной работы их указывать не надо, но если не нравятся значения по умолчанию их можно указать свои.

#### Инпуты:

#### Названия этапов:

##### <span style="color: rgb(53, 152, 219);">**Пользовательские настройки**</span>

##### StageStartModeName

**Описание:** Название этапа для использования компоненты "RX StartMode"  
**Значение по умолчанию:** "Ручной старт"  
**Обязательность:** Нет

##### StageCheckVariablesName

**Описание:** Название этапа для использования компоненты "RX CheckVariables"  
**Значение по умолчанию:** "Проверка переменных"  
**Обязательность:** Нет

##### StagePreparedServerName

**Описание:** Название этапа для использования компоненты "RX PreparedServer"  
**Значение по умолчанию:** "Подготовка сервера"  
**Обязательность:** Нет

##### StageBuildPackageName

**Описание:** Название этапа для использования компоненты "RX BuildPackage"  
**Значение по умолчанию:** "Сборка пакета"  
**Обязательность:** Нет

##### StageDeployDTPackageName

**Описание:** Название этапа для использования компоненты "DeployDTPackage"  
**Значение по умолчанию:** "Публикация (DT)"  
**Обязательность:** Нет

##### StageUploadPackageName

**Описание:** Название этапа для использования компоненты "RX UploadPackage"  
**Значение по умолчанию:** "Выгрузка в облако"  

**Обязательность:** Нет
