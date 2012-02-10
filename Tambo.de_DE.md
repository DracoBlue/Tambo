# Tambo

Willkommen zu Tambo. Tambo ist ein Tutorial, was Dir nach und nach beibringt, wie Du mit Agavi eine neue Anwendung bauen kannst.

Dieses Dokument ist copyright by DracoBlue <http://dracoblue.net> 2010-2012.

## Das neue Agavi Projekt

Ein neues Agavi Projekt anlegen, ist sehr einfach.

Du legst einen neuen Ordner an

    $ mkdir tambo
    $ cd tambo

und erstellst darin einen Order `/libs`.

    $ mkdir libs
    $ cd libs

Darin lädst Du Dir eine aktuelle Version von Agavi:

    $ svn export http://svn.agavi.org/tags/1.0.7/src/ agavi

Nun brauchst Du noch die Dispatcher-Datei agavi:

    $ cd ..
    $ svn export http://svn.agavi.org/tags/1.0.7/bin/agavi-dist agavi
    $ chmod u+x agavi

Dann müssen wir noch den Pfad zu unserem lokalen agavi anpassen, dafür öffnen wir die agavi-Datei

    $ vim agavi

und ersetzen:

    AGAVI_SOURCE_DIRECTORY="@PEAR-DIR@/agavi"

mit

    AGAVI_SOURCE_DIRECTORY="`dirname $0`/libs/agavi"

Jetzt kannst du endlich das Projekt mit anlegen:

    $ ./agavi project
    Agavi > project-wizard:
    Agavi > project-create:
    
    Project base directory [/home/jan/tambo]:
    [property] Loading /home/jan/tambo/build.properties
    [property] Unable to find property file: /home/jan/tambo
    build.properties... skipped

dann kommt die Frage nach dem Projektname:

    Project name [New Agavi Project]: *Tambo Community Site*

wir antworten mit *Tambo Community Site*. Jetzt gibt es noch ein Projektprefix. Hier macht es sich gut (zwecks Copy+Paste) dort *Project* zu verwenden:

    Project prefix (used, for example, in the project base action) [Tambocommunitysite]: Project
    Default template extension [php]:
     [property] Loading /home/jan/tambo/build.properties
         [copy] Copying 1 file to /home/jan/tambo
    [filter:ReplaceTokens] Replaced "%%PROJECT_NAME%%" with "Tambo Community Site"
        [mkdir] Created dir: /home/jan/tambo/app
        [mkdir] Created dir: /home/jan/tambo/app/config
        [mkdir] Created dir: /home/jan/tambo/app/modules
        [mkdir] Created dir: /home/jan/tambo/app/models
        [mkdir] Created dir: /home/jan/tambo/app/cache
        [mkdir] Created dir: /home/jan/tambo/app/lib
        [mkdir] Created dir: /home/jan/tambo/app/log
        [mkdir] Created dir: /home/jan/tambo/app/templates
        [mkdir] Created dir: /home/jan/tambo/dev
        [mkdir] Created dir: /home/jan/tambo/dev/pub
        [mkdir] Created dir: /home/jan/tambo/pub
        [chmod] Changed file mode on '/home/jan/tambo/app/cache' to 777
        [chmod] Changed file mode on '/home/jan/tambo/app/log' to 777
         [copy] Copying 1 file to /home/jan/tambo/app
    [filter:ReplaceTokens] Replaced "%%AGAVI_SOURCE_LOCATION%%" with "/home/jan/tambo/./libs/agavi"
         [copy] Copying 3 files to /home/jan/tambo/app/lib
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
     [copy] Copying 13 files to /home/jan/tambo/app/config
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_PREFIX%%" with "Project"
    [filter:ReplaceTokens] Replaced "%%PROJECT_NAME%%" with "Tambo Community Site"
    [filter:ReplaceTokens] Replaced "%%TEMPLATE_EXTENSION%%" with "php"
         [copy] Copying 2 files to /home/jan/tambo/dev/pub
     [property] Loading /home/helma/tambo/build.properties
    [agavi.import] Could not read /home/helma/tambo/build.xml: Error reading project file [wrapped: property (unknown) doesn't support the 'userproperty' attribute.] (skipping)
    
    Agavi > project-locate:
    
     [property] Loading /home/helma/tambo/build.properties
    
    Agavi > public-create:

Nun fragt Agavi nach dem environment-Namen (*development-jan* passt):

    Name of the environment to bootstrap in dispatcher scripts [development]: development-jan
Should an Apache .htaccess file with rewrite rules be generated (y/n) [n]? 
     [copy] Copying 1 file to /home/helma/tambo/pub
     
Nun kommen sehr viele Fragen. Drücke jedes mal Enter. Wir werden immer den Default-Fall nehmen.

Irgendwann sehen wir das Terminal wieder.

    Agavi > project:
    $

Wir müssen die generierte config_handlers.xml von Agavi leider anpassen.

    $ vim app/config/config_handlers.xml
    
und ersetzen sie mit folgendem:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" xmlns="http://agavi.org/agavi/config/parts/config_handlers/1.0" parent="%core.system_config_dir%/config_handlers.xml">
        <ae:configuration>
            <handlers>
    
                <handler pattern="%core.module_dir%/*/*.validate.xml" class="AgaviValidatorConfigHandler">
                    <validation>%core.agavi_dir%/config/xsd/validators.xsd</validation>
                    <transformation>%core.agavi_dir%/config/xsl/validators.xsl</transformation>
                </handler>
    
                <handler pattern="%core.module_dir%/*/*.cache.xml" class="AgaviCachingConfigHandler">
                    <validation>%core.agavi_dir%/config/xsd/caching.xsd</validation>
                    <transformation>%core.agavi_dir%/config/xsl/caching.xsl</transformation>
                </handler>
    
            </handlers>
        </ae:configuration>
    
    </ae:configurations>

Wir löschen pub und dev/pub

    $ rm pub -rf
    $ rm dev/pub -rf

Die beiden Verzeichnisse sind beim Standard-Agavi dabei. Damit unsere Modul-Struktur nachher konsistent ist, werden wir das pub-Verzeichnis jedoch woanders anlegen.    
Wir legen nun unter app ein neues Verzeichnis mit dem Namen pub an

    $ mkdir app/pub
    
und darin eine Datei index.php index.php

    $ vim app/pub/index.php
    
mit folgendem Inhalt an

    <?php
    error_reporting(E_ALL | E_STRICT);
    $app_dir = dirname(dirname(__FILE__)) . '/';
    require($app_dir . '../libs/agavi/agavi.php');
    require($app_dir . 'config.php');
    unset($app_dir);
    
    Agavi::bootstrap('development-jan');
    AgaviConfig::set('core.default_context', 'web');
    AgaviContext::getInstance()->getController()->dispatch();
    
Unter http://localhost/tambo/app/pub/ ist bereits die Willkommenseite sichtbar.

## Das erste Agavi-Modul anlegen

Als erstes wollen wir den News-Bereich für unsere Community-Seite fertigstellen. Deswegen legen wir ein neues Modul an.

    $ mkdir app/modules/News
    $ cd app/modules/News

Bevor wir anfangen können Actions und Views zu definieren, brauchen wir eine module.xml.

Dafür legen wir in unserem neuen News-Modul ein Verzeichnis config an

    $ mkdir config

und tun dort rein eine module.xml

    $ vim config/module.xml
    
mit folgendem Inhalt

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/module/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0">
        <ae:configuration>
            <module enabled="true">
                <settings>
                    <setting name="title">News Module</setting>
                    <setting name="version">1.0</setting>
                    <setting name="agavi.action.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}Action.class.php</setting>
                    <setting name="agavi.cache.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.cache.xml</setting>
                    <setting name="agavi.template.directory">%core.module_dir%/${module}/impl/</setting>
                    <setting name="agavi.validate.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.validate.xml</setting>
                    <setting name="agavi.view.path">%core.module_dir%/${moduleName}/impl/${viewName}View.class.php</setting>
                    <setting name="agavi.view.name">${actionName}/${actionName}${viewName}</setting>
                </settings>
            </module>
        </ae:configuration>
    </ae:configurations>

Nun müssen wir für die impl-struktur auch noch einen Ordner anlegen.

    $ mkdir impl
    $ cd impl

Unser Newsbereich braucht hat am Anfang nur eine einzige Action.

Im impl-Ordner legen wir daher einen Ordner mit dem Namen der Action an.

    $ mkdir ListNews
    $ cd ListNews

Nun erstellen wir darin unsere Action-Datei

    $ vim ListNewsAction.class.php
    
mit folgendem Inhalt

    <?php
    class News_ListNewsAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            return "Success";
        }
    
    }

Da jede Action wenigstens einen View braucht, legen wir auch noch einen View

    $ vim ListNewsSuccessView.class.php
    
mit folgendem Inhalt

    <?php
    class News_ListNews_ListNewsSuccessView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->setupHtml($rd);
        }
    
    }
    
an.

Nun legen wir noch ein Template an

    $ vim ListNewsSuccess.php
    
Dieses hat den Inhalt

    <ul>
        <li>Hallo</li>
    </ul>

Damit die Action gefunden wird, tragen wir sie in der routing.xml ein.

Dazu gehen wir zurück ins root-Verzeichnis der Anwendung

    $ cd ../../../../../
    
und ändern die routing.xml im app/config Ordner

    $ vim app/config/routing.xml

Wir ersetzen die Datei einfach mit folgendem Inhalt:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" xmlns="http://agavi.org/agavi/config/parts/routing/1.0">
        <ae:configuration>
            <routes>
                <route name="news" pattern="^/news$" module="News">
                    <route name=".list" pattern="^$" action="ListNews" />
                </route>
            </routes>
        </ae:configuration>
    </ae:configurations>

Wenn wir nun auf <http://localhost/tambo/index.php/news/> surfen, sehen wir den Newsbereich. Er enthält derzeit nur den Text "Hallo". Bravo!

## Der NewsService

Unser Tor zu den News-Daten wird unser NewsService sein. Das müssen wir nun erstmal erstellen.

Dafür gehen wir in den Ordner der models

    $ cd app/models

und legen darin eine neue Datei NewsServiceModel.class.php

    $ vim models/NewsServiceModel.class.php
    
mit folgendem Inhalt an

    <?php
    class NewsServiceModel {
    
        private $news_mock_data = array(
            1 => array(
                'title' => 'Second Post!',
                'id' => 1,
                'body' => 'Yeah and an other one!'
            ),
            2 => array(
                'title' => 'New Site: Powered by Agavi',
                'id' => 2,
                'body' => 'This is our new site and guess what, it is powered by agavi!'
            )
        );
            
        public function getNews($offset, $limit) {
            return $this->news_mock_data;
        }
        
    }

Nun passen wir die ListNewsAction an

    $ vim impl/ListNews/ListNewsAction.class.php
    
wir erweitern sie so, dass sie wie folgt aussieht:

    <?php
    class News_ListNewsAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('NewsService');
    
            $this->setAttribute('news_posts', $news_model->getNews(0, 10));
            return "Success";
        }
    
    }

durch das setzen des Attributs, wird die Variable $t['news_posts'] ab jetzt auch im Template verfügbar sein.

Wir erweitern deshalb nun das Template NewsListSuccess.php wie folgt:

    <?php
    if (count($t['news_posts']) === 0) {
    ?>
    Es gibt noch keine News.    
    <?php
    } else {
    ?>
    <ul>
    <?php
        foreach ($t['news_posts'] as $news_post) {
    ?>
        <li>
            <h2><?php echo htmlspecialchars($news_post['title']); ?></h2>
            <div><?php echo htmlspecialchars($news_post['body']); ?></div>
        </li>
    <?php
        }
    ?>
    </ul>
    <?php
    }
    ?>

Wenn wir nun auf <http://localhost/tambo/index.php/news/> surfen, sehen wir unsere 2 Newsposts.




## Detailseiten für jeden Newseintrag

Nun ändenr wir die routing.xml im app/config Ordner

    $ vim app/config/routing.xml

Wir erweitern die routing.xml, so dass sie danach so aussieht:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" xmlns="http://agavi.org/agavi/config/parts/routing/1.0">
        <ae:configuration>
            <routes>
                <route name="news" pattern="^/news$" module="News">
                    <route name=".list" pattern="^$" action="ListNews" />
                    [strong]<route name=".item" pattern="^view/(news_item:\d+)$" action="ViewNewsItem" />[/strong]
                </route>
            </routes>
        </ae:configuration>
    </ae:configurations>

Die neue freigeschaltene URL ist /news/view/2 (wobei 2 jede andere beliebige url sein kann). Die URL funktioniert aber noch nicht richtig, weil wir noch Action+View und Template anlegen müssen.

Dazu gehen wir in ins News-Module

    $ cd app/modules/News/
    
und legen im impl-Ordner eine neues Verzeichnis ViewNewsItem an

    $ mkdir impl/ViewNewsItem
    $ cd impl/ViewNewsItem

Wir legen wieder die 3 Dateien für ViewNewsItem analog zu ListViews an.

Die View:

    $ vim ViewNewsItemSuccessView.class.php

sieht so aus:

    <?php
    
    class News_ViewNewsItem_ViewNewsItemSuccessView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->setupHtml($rd);
        }
    
    }

Das Template

    $ vim ViewNewsItemSuccess.class.php
    
sieht so aus:

    <?php
        $news_post = $t['news_post'];
    ?>
    <h1><?php echo htmlspecialchars($news_post['title']); ?></h1>
    <div><?php echo htmlspecialchars($news_post['body']); ?></div>

Diesmal als letztes die Action (weil wir damit was spezielles Vorhaben)

    $ vim ViewNewsItemAction.class.php
    
mit folgendem Inhalt:

    <?php
    class News_ViewNewsItemAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('NewService');
    
            try {
                $this->setAttribute('news_item', $news_model->getNewsItem($rd->getParameter('news_item')));
            } catch (Exception $e) {
               return array('Default', 'Error404Success');
            }
    
            return "Success";
        }
    
    }

Wie Du sehen kannst, erwartet dieser Code, dass es eine getNewsItem Methode auf dem NewServiceModel gibt. Diese pflegen wir nun nach.

Öffne die app/models/NewServiceModel.class.php und füge die Methode getNewsItem mit folgendem Inhalt hinzu:

    function getNewsItem($id) {
        if (!isset($this->news_mock_data[$id])) {
            throw new Exception('News with id #' . $id . ' does not exist!');
        }
        return $this->news_mock_[$id];
    }
    
Leider wird die Url <http://localhost/tambo/index.php/news/view/1/> immer noch nicht funktionieren. Der Grund dafür ist die Validierung.

Wir legen deswegen unter app/modules/News/impl/ViewNewsItem noch eine Datei ViewNewsItem.validate.xml an.

    $ vim ViewNewsItem.validate.xml
    
Sie hat folgenden Inhalt:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/validators/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" parent="%core.module_dir%/News/config/validators.xml" >
        <ae:configuration>
            <validators>
                <validator class="number">
                    <argument>news_item</argument>
                </validator>
            </validators>
        </ae:configuration>
    </ae:configurations>

Diese Datei ist dafür zuständig die Daten, die wir reinbekommen (in diesem Fall die news_item ID) zu validieren. In Agavi kommt kein Wert bis zur Action, wenn er nicht validiert ist. Das hat den Vorteil, dass man so nur bei sehr fahrlässiger Programmierung in Probleme wie SQL-Injection oder andere Sicherheitsprobleme reinrennt.

Da wir auch noch eine Verlinkung für die Detailansicht wollen, gehen wir nun in unser NewsServiceModel und spendieren der getNews-Methode noch die URL-Generierung.

Dazu öffnen wir nun die NewsServiceModel.class.php

    $ vim app/models/NewsServiceModel.class.php
    
und erweitern den Inhalt wie folgt:

    <?php
    class NewsServiceModel {
    
        private $news_mock_data = array(
            1 => array(
                'title' => 'Second Post!',
                'id' => 1,
                'body' => 'Yeah and an other one!'
            ),
            2 => array(
                'title' => 'New Site: Powered by Agavi',
                'id' => 2,
                'body' => 'This is our new site and guess what, it is powered by agavi!'
            )
        );
    
        public function getNews($offset, $limit) {
            $results = array();
    
            foreach ($this->news_mock_data as $result) {
                $result['url'] = $this->getContext()->getRouting()->gen(
                    'news.item',
                    array(
                        'news_item' => $result['id']
                    )
                );
                $results[] = $result;
            }
    
            return $results;
        }
    
        function getNewsItem($id) {
            if (!isset($this->news_mock_data[$id])) {
                throw new Exception('News with id #' . $id . ' does not exist!');
            }
    
            $result = $this->news_mock_data[$id];
            $result['url'] = $this->getContext()->getRouting()->gen('news.item', array('news_item' => $result['id']));
    
            return $result;
        }
    
    }

Wenn wir nun unser Template ListNewsSuccess.php

    $ vim app/modules/News/impl/ListNews/ListNewsSuccess.php

wie folgt anpassen

    <?php
    if (count($t['news_posts']) === 0) {
    ?>
    Es gibt noch keine News.    
    <?php
    } else {
    ?>
    <ul>
    <?php
        foreach ($t['news_posts'] as $news_post) {
    ?>
        <li>
            <h2>[strong]<a href="<?php echo $news_post['url']; ?>">[/strong]<?php echo htmlspecialchars($news_post['title']); ?>[strong]</a>[/strong]</h2>
            <div><?php echo htmlspecialchars($news_post['body']); ?></div>
        </li>
    <?php
        }
    ?>
    </ul>
    <?php
    }
    ?>

können wir unter <http://localhost/tambo/index.php/news/> sogar auf die einzelnen Newseinträge draufklicken.



## Datenanbindung mit Doctrine

Wir wollen die News selbstverständlich nicht weiterhin hartkodiert in unserem NewsServiceModel haben. Deswegen werden wir nun Mysql via Doctrine anbinden.

### Datenbank in Agavi aktivieren

Standardgemäß ist die Datenbank bei Agavi deaktiviert. Deswegen müssen wir sie in der settings.xml

    $ vim app/config/settings.xml

aktivieren:
            <setting name="use_database">[strong]true[/strong]</setting>

### Doctrine installieren/einrichten

Dazu gehen wir zurück ins root Verzeichnis unserer Anwendung. Danach begeben wir uns in das libs Verzeichnis

    $ cd libs

und checken uns die aktuelle Version von Doctrine aus

    $ svn export http://svn.doctrine-project.org/tags/1.2.1/lib doctrine
    
und gehen zurück ins root-Verzeichnis der Anwendung

    $ cd ..

Nun müssen wir die databases.xml anpassen:

    $ vim app/config/databases.xml
    
und ersetzen den Inhalt mit folgendem:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" xmlns="http://agavi.org/agavi/config/parts/databases/1.0">
    
        <ae:configuration environment="development-jan">
            <databases default="db">
                <database name="db" class="AgaviDoctrineDatabase">
                    <ae:parameters>
                        <ae:parameter name="dsn">mysql://tambo:8MTMbdvncbjfBJuf@127.0.0.1:3306/tambo</ae:parameter>
                        <ae:parameter name="attributes">
                            <ae:parameters>
                                <ae:parameter name="Doctrine_Core::ATTR_AUTOLOAD_TABLE_CLASSES">true</ae:parameter>
                                <ae:parameter name="Doctrine_Core::ATTR_VALIDATE">LENGTHS</ae:parameter>
                                <ae:parameter name="Doctrine_Core::ATTR_AUTO_ACCESSOR_OVERRIDE">true</ae:parameter>
                            </ae:parameters>
                        </ae:parameter>
                        <ae:parameter name="manager_attributes">
                            <ae:parameters>
                                <ae:parameter name="Doctrine_Core::ATTR_MODEL_LOADING">conservative</ae:parameter>
                            </ae:parameters>
                        </ae:parameter>
                        <ae:parameter name="load_models">%core.lib_dir%/doctrine</ae:parameter>
                        <ae:parameter name="charset">UTF8</ae:parameter>
                    </ae:parameters>
                </database>
            </databases>
        </ae:configuration>
    
    </ae:configurations>

Damit doctrine den models Ordner auch finden wird, erzeugen wir ihn

    $ mkdir app/lib/doctrine


Zusätzlich müssen wir Agavi noch sagen, wo die Doctrine Klasse liegt. Das machen wir in der systemweiten Autoload-Konfiguration unter autoload.xml

    $ vim app/config/autoload.xml
    
und wir ersetzen den Inhalt hiermit:

    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/autoload/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" parent="%core.system_config_dir%/autoload.xml">
        <ae:configuration>
    
            [strong]<autoload name="Doctrine">%core.app_dir%/../libs/doctrine/Doctrine.php</autoload>[/strong]
    
            <autoload name="ProjectBaseAction">%core.lib_dir%/action/ProjectBaseAction.class.php</autoload>
            <autoload name="ProjectBaseModel">%core.lib_dir%/model/ProjectBaseModel.class.php</autoload>
            <autoload name="ProjectBaseView">%core.lib_dir%/view/ProjectBaseView.class.php</autoload>
    
        </ae:configuration>
    </ae:configurations>

Um Doctrine benutzen zu können, brauchen wir ein doctrine dispatcher script (analog zu dem agavi script). Das nennen wir doctrine und es wird sich neben dem agavi script befinden

    $ vim doctrine

und folgenden Inhalt haben:

    #!/usr/bin/env php
    <?php
    $args = $_SERVER['argv'];
    
    $app_dir = realpath(dirname($args[0]));
    
    require($app_dir.'/libs/agavi/agavi.php');
    require($app_dir.'/app/config.php');
    
    $env = 'development-jan';
    
    Agavi::bootstrap($env);
    
    AgaviConfig::set('core.default_context', 'console');
    $con = AgaviContext::getInstance()->getDatabaseConnection();
    
    $config = array(
        'data_fixtures_path'  => AgaviConfig::get('doctrine.fixture_dir', $app_dir . '/dev/data/fixtures'),
        'models_path' => AgaviConfig::get('core.lib_dir') . '/doctrine',
        'migrations_path' =>  AgaviConfig::get('doctrine.migration_dir', $app_dir . '/dev/migrations'),
        'sql_path' => AgaviConfig::get('doctrine.migration_dir', $app_dir . '/dev/data/sql'),
        'yaml_schema_path' =>  AgaviConfig::get('doctrine.schema_dir', $app_dir . '/dev/schema/schema.yml'),
        'generate_models_options' => array(
            'suffix' => '.class.php'
        )
    );
    
    $cli = new Doctrine_Cli($config);
    $cli->run($args);

Das Script muss auch noch ausführbar gemacht werden:

    $ chmod u+x doctrine

Ein Aufruf von ./doctrine zeigt uns alle Optionen:

    $ ./doctrine
    Doctrine Command Line Interface
    
    ./doctrine dql
    ./doctrine generate-migrations-diff
    ./doctrine generate-models-db
    ./doctrine generate-yaml-db
    ./doctrine create-tables
    ./doctrine build-all-load
    ./doctrine generate-migration
    ./doctrine compile
    ./doctrine build-all-reload
    ./doctrine generate-sql
    ./doctrine generate-migrations-db
    ./doctrine generate-migrations-models
    ./doctrine dump-data
    ./doctrine drop-db
    ./doctrine generate-yaml-models
    ./doctrine build-all
    ./doctrine generate-models-yaml
    ./doctrine migrate
    ./doctrine create-db
    ./doctrine rebuild-db
    ./doctrine load-data

### Schema anlegen

Wir brauchen nun ein Verzeichnis für unser Schema, es wird sich unter dev/schema/schema.yml befinden. Deswegen legen wir diese an

    $ mkdir dev/schema
    $ vim dev/schema/schema.yml

und füllen sie mit dem folgenden Inhalt

    NewsItem:
      tableName: news_items
      columns:
        id:
          type: integer(8)
          unsigned: 1
          primary: true
          autoincrement: true
        body:
          type: string(2147483647)
          notnull: true
        title: string(255)

Wichtig: Bei Yaml-Dateien werden immer Leerzeichen zum Einrücken verwendet. 

Nun generieren wir die ORM-Models:

    $ ./doctrine generate-models-yaml
    generate-models-yaml - Generated models successfully from YAML schema
    
und erzeugen die Datenbank:

    ./doctrine create-tables
    create-tables - Created tables successfully

### Testdaten

Da wir initial ein paar Testdaten (sogenannte Fixtures) haben wollen, legen wir eine news_items.yml im fixtures Ordner an:

    $ mkdir dev/fixtures
    $ vim dev/fixtures/news_items.yml
    
mit folgendem Inhalt

    NewsItem:
      FirstPost:
        id: 1
        title: This is my First Post!
        body: This is my very first post
      SecondPost:
        id: 2
        title: Second Post!
        body: And another one!!
    
In die Datenbank laden wir die Daten so:

    $ ./doctrine load-data
    load-data - Data was successfully loaded

### Das NewsServiceModel dynamisieren

Das NewsServiceModel muss nun um einiges angepasst werden, das Ergebnis sieht dann so aus:

    <?php
    class NewsServiceModel {
    
        private function getNewsItemAsArray($news_item) {
            return array(
                'id' => $news_item->id,
                'title' => $news_item->title,
                'body' => $news_item->body,
                'url' => $this->getContext()->getRouting()->gen(
                    'news.item',
                    array(
                        'news_item' => $news_item->id
                    )
                )
            );
        }
    
    
        public function getNews($offset, $limit) {
            $results = array();
    
            $query = new Doctrine_Query();
            $query->from('NewsItem');
            $query->limit($limit);
            $query->offset($offset);
    
    
            foreach ($query->execute() as $news_item) {
                $results[] = $this->getNewsItemAsArray($news_item);
            }
    
            return $results;
        }
    
        function getNewsItem($news_item_id) {
    
            $query = new Doctrine_Query();
            $query->from('NewsItem')->where('id = ?', $news_item_id);
    
            $news_item = $query->fetchOne();
    
            if (!$news_item) {
                throw new Exception('News with id #' . $news_item_id . ' does not exist!');
            }
    
            return $this->getNewsItemAsArray($news_item);
        }
    
    }

Unter <http://localhost/tambo/index.php/news/> können wir nun sogar unsere Newsposts aus der Datenbank sehen! Bravo!
    
## Einloggen
    
### Das User-Modul
    
Wir legen erstmal ein neues Modul "User" an:
    
    $ mkdir app/modules/User
    $ mkdir app/modules/User/config
    $ mkdir app/modules/User/impl
    
Damit das Module auch richtig funktioniert, erzeugen wir die module.xml für das Modul:
    
    $ vim app/modules/User/config/module.xml
    
mit folgendem Inhalt
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/module/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0">
        <ae:configuration>
            <module enabled="true">
                <settings>
                    <setting name="title">User Module</setting>
                    <setting name="version">1.0</setting>
                    <setting name="agavi.action.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}Action.class.php</setting>
                    <setting name="agavi.cache.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.cache.xml</setting>
                    <setting name="agavi.template.directory">%core.module_dir%/${module}/impl/</setting>
                    <setting name="agavi.validate.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.validate.xml</setting>
                    <setting name="agavi.view.path">%core.module_dir%/${moduleName}/impl/${viewName}View.class.php</setting>
                    <setting name="agavi.view.name">${actionName}/${actionName}${viewName}</setting>
                </settings>
            </module>
        </ae:configuration>
    </ae:configurations>
    
### UserServiceModel
    
Bevor wir jegliches Login machen können, brauchen wir etwas was das Login validiert. Deswegen bauen wir analog zum NewsServiceModel nun auch ein UserServiceModel.

Dafür erzeugen wir eine neue Datei UserServiceModel.class.php:

    $ vim app/models/UserServiceModel.class.php
    
mit folgendem Inhalt:

    <?php
    class UserServiceModel {
    
        private $user_mock_data = array(
            1 => array(
                'email' => 'jans@dracoblue.de',
                'id' => 1,
                'name' => 'Draco',
                'password' => '1234'
            ),
            2 => array(
                'email' => 'kontakt@webdevberlin.com',
                'id' => 2,
                'name' => 'Jan',
                'password' => '23'
            )
        );
    
        public function getUserByEmailAndPassword($email, $password) {
            $email = strtolower($email);
            foreach ($this->user_mock_data as $user) {
                if ($user['email'] === $email && $user['password'] === $password) {
                    unset($user['password']);
                    return $user;
                }
            }
            throw new Exception('User with the email ' . $email . ' and that password not found!');
        }
    
        public function getUser($user_id) {
            if (!isset($this->user_mock_data[$user_id])) {
                throw new Exception('User with id ' . $user_id . ' not found!');
            }
    
            $user = $this->user_mock_data[$user_id];
            unset($user['password']);
    
            return $user;
        }
    
    }
    
### Der Userbar-Slot
    
Für den User soll oben eine Leiste angezeigt werden, wo er sich entweder einloggen kann, oder sieht, dass er eingeloggt ist.

Diese Userbar wird in Agavi idealerweise als Slot realisiert. Da ein Slot nur der Aufruf einer Action ist, registrieren wir unsere neue Action im User-Modul mit dem Namen UserbarAction.class.php

Dafür legen wir das Action-Verzeichnis an

    $ mkdir app/modules/User/impl/UserBar
    $ cd app/modules/User/impl/UserBar
    
Nun kommt die Logik für die Action in

    $ vim UserBarAction.class.php

ist diese:
    
    <?php
    class User_UserBarAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            $session = $this->getContext()->getUser();
    
            if ($session->isAuthenticated()) {
                $this->setAttribute('user_info',
                    array(
                        'name' => $session->getAttribute('name'),
                        'email' => $session->getAttribute('email'),
                        'id' => $session->getAttribute('id')
                    )
                );
                
                $this->setAttribute('logout_url', $this->getContext()->getRouting()->gen('user.logout'));
                
                return "LoggedIn";
            }
            
            $this->setAttribute('login_url', $this->getContext()->getRouting()->gen('user.login'));
             
            return "Login";
        }
    
    }
    
Von der Action wird entweder die LoggedIn oder die Login-View zurückgegeben.
    
Die LoggedIn-View unter UserBarLoggedInView.class.php

    vim UserBarLoggedInView.class.php[/code]

ist die folgende:

    <?php
    
    class User_UserBar_UserBarLoggedInView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->setupHtml($rd);
        }
    
    }
    
Die Login-View

    $ vim UserBarLoginView.class.php

    sieht analog dazu, so aus:

    <?php
    
    class User_UserBar_UserBarLoginView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->setupHtml($rd);
        }
    
    }
    
Interessant wird es erst wieder im Template. Das Login-Template

    $ vim UserBarLogin.php

sieht wie folgt aus:

    <form method="post" action="<?php echo $t['login_url']; ?>">
        Hallo Gast. Bitte logge Dich ein.
        <label for="login-email">
            Email:
        </label>
        <input name="login-email" type="text" value=""/>
        <label for="login-password">
            Passwort:
        </label>
        <input name="login-password" type="password" value=""/>
    
        <input type="submit" value="Einloggen" />
    </form>
    
Das LoggedIn-Template
    
    vim UserBarLoggedIn.php

sieht dann so aus:

    <?php
        $user_info = $t['user_info'];
    ?>
    Hallo <?php echo htmlspecialchars($user_info['name']); ?>. <a href="<?php echo $t['logout_url']; ?>">Ausloggen</a>?
    
Nun haben wir zwar eine schöne Action, aber sie wird nie aufgerufen und auch nicht angezeigt. Der Weg geht nun aber nicht über die routing.xml, sondern über die output_types.xml!
    
Wir gehen zurück in das root-Verzeichnis.

    $ cd ../../../../../
    
Und passen die output_types.xml an

    $ vim app/config/output_types.xml
    
Der Bereich layouts-standard enthält ein layer mit dem Namen "decorator", dort fügen wir folgendes ein:

    <slot name="user_bar" module="User" action="UserBar" />
    
Damit der Slot auch im Master-Template angezeigt wird, fügen wir kurz nach dem <body> in app/templates/Master.php

    $ vim app/templates/Master.php

die folgenden Zeilen ein:

        <div>
            <?php echo $slots['user_bar']; ?>
        </div>
    
Wenn wir nun eine beliebige Seite unserer Community Seite anschauen, sehen wir oben immer den Balken, dass wir uns doch bitte einloggen möchten.

## Noch nicht zu Markdown konvertiert ... work in progress ...
    
    [h2]Die Login/Logout-Seite[/h2]
    
    Wir brauchen zusätzlich noch eine Login-Action.
    
    Dazu gehen wir nochmal in das User-Modul und legen einen Ordner für die neue Action an.
    [code]$ mkdir app/modules/User/impl/Login
    $ cd app/modules/User/impl/Login[/code]
    
    Eine neue route zum Login-Formular und der Logout-Seite wird in der routing.xml angelegt
    [code]$ vim ../../../../config/routing.xml[/code]
    in dem folgender Inhalt vor das letzte </routes> eingefügt wird:
    [code]            <route name="user" pattern="^/user/" module="User">
                    <route name=".login" pattern="^login/$" action="Login" />
                    <route name=".logout" pattern="^logout/$" action="Logout" />
                </route>
    [/code]
    
    Wir definieren nun die LoginAction
    [code]$ vim LoginAction.class.php[/code]
    mit dem Inhalt:
    [code]<?php
    class User_LoginAction extends ProjectBaseAction {
    
        function executeRead(AgaviRequestDataHolder $rd) {
            return 'Input';
        }
        
        function handleError(AgaviRequestDataHolder $rd) {
            $this->setAttribute('error_message', 'Please provide email and password!');
            return 'Input';
        }
    
        function executeWrite(AgaviRequestDataHolder $rd) {
            $login_email = $rd->getParameter('login-email');
            $login_password = $rd->getParameter('login-password');
    
            $session = $this->getContext()->getUser();
    
            $users_model = $this->getContext()->getModel('Users', 'User');
    
            $user = null;
            try {
                $user = $users_model->getUserByEmailAndPassword($login_email, $login_password);
            } catch (Exception $e) {
                $this->setAttribute('error_message', $e->getMessage());
                return 'Input';
            }
    
            $session->setAuthenticated(true);
            $session->setAttribute('name', $user['name']);
            $session->setAttribute('id', $user['id']);
            $session->setAttribute('email', $user['email']);
    
            return "Success";
        }
    
    }[/code]
    
    Damit sowohl Email als auch das Passwort bis zur Action durchkommen, müssen sie validiert werden.
    
    Dafür brauchen wir die Login.xml
    [code]$ vim Login.xml[/code]
    welche dann so aussieht:
    [code]<?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/validators/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" parent="%core.config_dir%/validators.xml" >
        <ae:configuration>
            <validators>
                <validator class="string">
                    <argument>login-email</argument>
                </validator>
                <validator class="string">
                    <argument>login-password</argument>
                </validator>
            </validators>
        </ae:configuration>
    </ae:configurations>[/code]
    
    Da wir im Fehlerfall beim Login nur die Fehlermeldung anzeigen wollen, geben wir diese auch nur aus. Das Login ist ja bereits in UserBar implementiert.
    
    Die
    [code]$ vim LoginInput.php[/code]
    sieht also so aus
    [code]<?php
        if (isset($t['error_message'])) {
    ?>
    <div>
        <?php echo htmlspecialchars($t['error_message']); ?>
    </div>
    <?php
        }
    ?>[/code]
    
    Der LoginInputView ist wie alle anderen nicht sehr überraschend
    [code]$ vim LoginInputView.class.php[/code]
    mit folgendem Inhalt
    [code]<?php
    
    class User_Login_LoginInputView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->setupHtml($rd);
        }
    
    }[/code]
    
    Dagegen wird [strong]keine[/strong] LoginSuccess.php angelegt, sondern [strong]nur[/strong] ein
    [code]$ vim LoginSuccessView.class.php[/code]
    mit diesem Inhalt:
    [code]<?php
    
    class User_Login_LoginSuccessView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->getResponse()->setRedirect($this->getContext()->getRouting()->gen('news.list'));
        }
    
    }[/code]
    
    [h2]Das Logout[/h2]
    
    Erstmal legen wir nun noch die Logout-Action an.
    [code]$ cd ..
    $ mkdir Logout
    $ cd Logout[/code]
    
    Die Logout-Action tut nicht viel besonderes, außer den User ausloggen.
    [code]$ vim LogoutAction.class.php[/code]
    mit dem folgenden Code:
    [code]<?php
    class User_LogoutAction extends ProjectBaseAction {
    
        function executeRead(AgaviRequestDataHolder $rd) {
            $session = $this->getContext()->getUser();
    
            $session->setAuthenticated(false);
    
            return "Success";
        }
    
    }[/code]
    
    Und dann macht
    [code]$ vim LogoutSuccessView.class.php[/code]
    auch wieder nur einen Redirect:
    [code]<?php
    
    class User_Logout_LogoutSuccessView extends ProjectBaseView {
    
        function executeHtml(AgaviRequestDataHolder $rd) {
            $this->getResponse()->setRedirect($this->getContext()->getRouting()->gen('news.list'));
        }
    
    }[/code]
    
    [h2]Das Login/Logout ausprobieren![/h2]
    
    Nun können wir uns wahlweise mit der Email jans@dracoblue.de und dem Passwort 1234 einloggen und danach wieder ausloggen. Unabhängig ob wir auf die Detailseite der News oder die News-Liste anschauen, der Status bleibt erhalten!
    
    
    
    [h1]UsersModel-Daten aus der Datenbank[/h1]
    
    Auch die User sollen selbstverständlich aus der Datenbank kommen. Deswegen holen wir diese wie die News per Doctrine aus einer Mysql Datenbank.
    
    [h2]Schema anpassen[/h2]
    
    Am Anfang passen wir die schema.yml an
    [code]$ vim dev/schema/schema.yml[/code]
    und fügen folgenden Inhalt ganz an das Ende:
    [code]User:
      tableName: users
      columns:
        id:
          type: integer(8)
          unsigned: 1
          primary: true
          autoincrement: true
        email:
          type: string(255)
        password:
          type: string(255)
        first_name:
          type: string(255)[/code]
    
    Nun generieren wir die ORM-Models:
    [code]$ ./doctrine generate-models-yaml
    generate-models-yaml - Generated models successfully from YAML schema[/code]
    und erzeugen die Datenbanktabellen:
    [code]./doctrine create-tables
    create-tables - Created tables successfully[/code]
    
    [h2]Testdaten anlegen[/h2]
    
    Da wir initial auch bei den Usern ein paar Testdaten (sogenannte Fixtures) haben wollen, legen wir eine users.yml im fixtures Ordner an:
    [code]$ mkdir dev/fixtures
    $ vim dev/fixtures/users.yml[/code]
    mit folgendem Inhalt
    [code]User:
      DracoBlue:
        id: 1
        email: JanS@DracoBlue.de
        first_name: Draco
        password: 1234
      WebDevBerlin:
        id: 2
        email: kontakt@webdevberlin.com
        first_name: Jan
        password: 23[/code]
        
    Danach laden wir die Daten wieder in die Datenbank:
    [code]$ ./doctrine load-data
    load-data - Data was successfully loaded[/code]
    
    [h2]Das UsersModel dynamisieren[/h2]
    
    Das UsersModel muss nun um einiges angepasst werden, das Ergebnis sieht dann so aus:
    [code]<?php
    class User_UsersModel extends ProjectBaseModel implements AgaviISingletonModel {
    
        public function getUserAsArray($user) {
            return array(
                'name' => $user->first_name,
                'email' => $user->email,
                'id' => $user->id
            );
        }
    
    
        public function getUserByEmailAndPassword($email, $password) {
            $email = strtolower($email);
    
            $query = new Doctrine_Query();
            $query->from('User')->where('email = ? AND password= ?', array($email, $password));
    
            $user = $query->fetchOne();
    
            if (!$user) {
                throw new Exception('User with the email ' . $email . ' and that password not found!');
            }
    
            return $this->getUserAsArray($user);
        }
    
        public function getUser($user_id) {
    
            $query = new Doctrine_Query();
            $query->from('User')->where('id = ?', $user_id);
    
            $user = $query->fetchOne();
    
            if (!$user) {
                throw new Exception('User with id ' . $user_id . ' not found!');
            }
    
            return $this->getUserAsArray($user);
        }
    
    }[/code]
    
    Unter http://localhost/tambo/index.php/news/ können wir uns nun sogar mit den Daten aus der Datenbank einloggen.
    
    Das Passwort ist noch nicht verschlüsselt! Unbedingt nicht so unverschlüsselt im Livebetrieb machen!
    
    
    [h1]Kommentare zu den News[/h1]
    
    [h2]Das Extras-Module[/h2]
    
    Bevor wir beginnen können, müssen wollen wir ein neues Module anlegen. Da wir auf lange Sicht neben Kommentaren auch noch Tags und andere kleine wiederverwendbare Teile realisieren wollen, legen wir uns diesmal ein Modul mit dem Namen "Extras" an:
    [code]$ mkdir app/modules/Extras
    $ mkdir app/modules/Extras/config
    $ mkdir app/modules/Extras/models
    $ mkdir app/modules/Extras/impl[/code]
    und füllen auch gleich wieder die config.xml
    [code]$ vim app/modules/Extras/config.module.xml[/code]
    mit folgendem Inhalt aus:
    [code]<?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/module/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0">
        <ae:configuration>
            <module enabled="true">
                <settings>
                    <setting name="title">Extras Module</setting>
                    <setting name="version">1.0</setting>
                    <setting name="agavi.action.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}Action.class.php</setting>
                    <setting name="agavi.cache.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.cache.xml</setting>
                    <setting name="agavi.template.directory">%core.module_dir%/${module}/impl/</setting>
                    <setting name="agavi.validate.path">%core.module_dir%/${moduleName}/impl/${actionName}/${actionName}.validate.xml</setting>
                    <setting name="agavi.view.path">%core.module_dir%/${moduleName}/impl/${viewName}View.class.php</setting>
                    <setting name="agavi.view.name">${actionName}/${actionName}${viewName}</setting>
                </settings>
            </module>
        </ae:configuration>
    </ae:configurations>[/code]
    
    [h2]Das CommentsModel[/h2]
    
    Da wir das CommentsModel für viele Arten von Kommentaren verwenden wollen, werden wir es etwas generisch gestalten. Auf den ersten Blick mag das vielleicht etwas kompliziert aussehen, aber schon bald wirst Du merken, dass wir uns dadurch viel Schreibarbeit sparen.
    
    Wir legen nun erstmal das CommentsModel.class.php an
    [code]$ vim CommentsModel.class.php[/code]
    mit dem Inhalt:
    [code]<?php
    class Extras_CommentsModel extends ProjectBaseModel implements AgaviISingletonModel {
    
        public function getCommentAsArray($comment) {
            $created_at = strtotime($comment->created_at);
            $ret_val = array(
                'id' => $comment->id,
                'body' => $comment->body,
                'created_at' => array(
                    'date' => strftime("%d/%m/%Y", $created_at),
                    'time' => strftime("%H:%M", $created_at)
                 )
            );
    
            $users_model = $this->getContext()->getModel('Users', 'User');
    
            if ($comment->user_id) {
                $ret_val['user'] = $users_model->getUser($comment->user_id);
            } else {
                $ret_val['user'] = $users_model->getGuestUser($comment->user_name);
            }
    
            return $ret_val;
        }
    
        public function getComments($entity_type, $entity_id, $offset, $limit) {
    
            $query = new Doctrine_Query();
            $query->from($entity_type . 'Comment');
            $query->where('entity_id = ?', $entity_id);
            $query->limit($limit);
            $query->offset($offset);
    
            $ret_val = array();
    
            foreach ($query->execute() as $comment) {
                $ret_val[] = $this->getCommentAsArray($comment);
            }
    
            return $ret_val;
        }
    
    }[/code]
    
    [h2]NewsComment-Datenbankstruktur erzeugen[/h2]
    
    Wie man erkennen kann, werden die Daten dynamisch von einer Doctrine-Datenbanktabelle mit dem Namen $EntityType + 'Comment' geholt. Am Anfang legen wir deswegen in unserer schema.yml
    [code]$ vim dev/schema/schema.yml[/code]
    durch hinzufügen dieser Zeilen an:
    [code]NewsComment:
      tableName: news_comments
      columns:
        id:
          type: integer(8)
          unsigned: 1
          primary: true
          autoincrement: true
        body:
          type: string(255)
        entity_id: integer(8)
        user_id: integer(8)
        user_name: string(255)
        created_at:
          type: timestamp[/code]
    
    [h2]NewsComment-Testdaten erzeugen[/h2]
    
    Wie immer wollen wir natürlich Testdaten für die Kommentare haben.
    
    Dafür passen wir diesmal die news_items.yml an
    [code]$ vim dev/fixtures/news_items.yml[/code]
    und fügen folgendes ganz ans Ende an
    [code]NewsComment:
      FirstComment:
        entity_id: 1
        body: This is my very first post
        user_id: 1
        user_name: 'DracoBlue'
        created_at: '2010-02-12 13:05'
      SecondComment:
        entity_id: 1
        body: Second Comment!
        user_name: 'Koala'
        created_at: '2010-02-12 15:05'[/code]
    
    [h2]Datenbank aktualisieren[/h2]
    
    Nun müssen wir erstmal die Datenbank aktualisieren, damit wir gleich auch die Daten sehen können.
    [code]$ ./doctrine generate-models-yaml
    generate-models-yaml - Generated models successfully from YAML schema
    $ ./doctrine create-tables
    create-tables - Created tables successfully
    $ ./doctrine load-data
    load-data - Data was successfully loaded[/code]
    
    [h2]Die Kommentare abholen[/h2]
    
    Die ViewNewsItem-Action muss erweitert werden.
    [code]$ vim app/modules/News/impl/ViewNewsItem/ViewNewsItemAction.class.php[/code]
    Sie enthält dann folgenden Inhalt:
    [code]<?php
    class News_ViewNewsItemAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('News', 'News');
            try {
                [strong]$news_item = $news_model->getNewsItem($rd->getParameter('news_item'));
                $this->setAttribute('news_item', $news_item);[/strong]
    
                $comments_model = $this->getContext()->getModel('Comments', 'Extras');
    
                [strong]$this->setAttribute('comments', $comments_model->getComments('News', $news_item['id'], 0, 10));[/strong]
            } catch (Exception $e) {
               return array('Default', 'Error404Success');
            }
    
            return "Success";
        }
    
    }[/code]
    
    Danach muss auch noch das Template unter ViewNewsItemSuccess.php
    [code]$ vim app/modules/News/impl/ViewNewsItem/ViewNewsItemSuccess.php[/code]
    wie folgt erweitert werden
    [code]<?php
        $news_item = $t['news_item'];
    [strong]    $comments = $t['comments'];[/strong]
    ?>
    <h1><?php echo htmlspecialchars($news_item['title']); ?></h1>
    <div><?php echo htmlspecialchars($news_item['body']); ?></div>
    [strong]<h2>Kommentare</h2>
    <ul>
    <?php
        foreach ($comments as $comment) {
    ?>
         <li>[<?php echo $comment['created_at']['time']; ?>] <?php echo htmlspecialchars($comment['user']['name']); ?>: <?php echo htmlspecialchars($comment['body']); ?></li>
    <?php
        }
    ?>
    </ul>[/strong][/code]
    
    
    [h1]Neue Kommentare erzeugen[/h1]
    
    [h2]Eine Form dem ViewNewsItem-Template hinzufügen[/h2]
    
    Wir öffnen die ViewNewsItemSuccess
    [code]$ vim app/modules/News/impl/ViewNewsItem/ViewNewsItemSuccess.php[/code]
    und erweitern den Code so:
    [code]<?php
        $news_item = $t['news_item'];
        $comments = $t['comments'];
    ?>
    <h1><?php echo htmlspecialchars($news_item['title']); ?></h1>
    <div><?php echo htmlspecialchars($news_item['body']); ?></div>
    <h2>Kommentare</h2>
    
    <form action="<?php echo $t['post_comment_url']; ?>" method="POST">
        <input type="hidden" name="news_item" value="<?php echo $news_item['id']; ?>" />
        <label for="body">Text</label>
        <textarea name="body"></textarea>
        <input type="submit" value="Abschicken" />
    </form>
    
    <ul>
    <?php
        foreach ($comments as $comment) {
    ?>
        <li>
            [<?php echo $comment['created_at']['time']; ?>]
            <?php echo htmlspecialchars($comment['user']['name']); ?>:
    
            <?php echo htmlspecialchars($comment['body']); ?>
        </li>
    <?php
        }
    ?>
    </ul>[/code]
    
    Nun generieren wir die Route noch richtig. Dafür müssen wir sie erstmal in der routing.xml registrieren:
    [code]$ vim app/config/routing.xml[/code]
    und ergänzen den route-Bereich für news wie folgt:
    [code]            <route name="news" pattern="^/news/" module="News">
                    [strong]<route name=".post_comment" pattern="^post-comment/$" action="PostNewsComment" />[/strong]
                    <route name=".list" pattern="^$" action="ListNews" />
                    <route name=".item" pattern="^view/(news_item:\d+)/$" action="ViewNewsItem" />
                </route>[/code]
    vor dem letzen </routes> an.
    
    Generieren tun wir die variable post_comment_url dann in der ViewNewsItemAction
    [code]$ vim app/modules/News/impl/ViewNewsItem/ViewNewsItemAction.class.php[/code]
    in dem wir folgendes anfügen:
    [code]<?php
    class News_ViewNewsItemAction extends ProjectBaseAction {
    
        function execute(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('News', 'News');
            try {
                $news_item = $news_model->getNewsItem($rd->getParameter('news_item'));
                $this->setAttribute('news_item', $news_item);
    
                $comments_model = $this->getContext()->getModel('Comments', 'Extras');
    
                $this->setAttribute('comments', $comments_model->getComments('News', $news_item['id'], 0, 10));
                [strong]$this->setAttribute('post_comment_url', $this->getContext()->getRouting()->gen('news.post_comment'));[/strong]
            } catch (Exception $e) {
               return array('Default', 'Error404Success');
            }
    
            return "Success";
        }
    
    }
    [/code]
    
    [h2]PostNewsComment-Action implementieren[/h2]
    
    In der routing.xml haben wir die PostNewsComment-Action bereits versprochen. Implementiert ist sie noch nicht, also machen wir das jetzt!
    [code]$ mkdir app/modules/News/impl/PostNewsComment
    $ vim app/modules/News/impl/PostNewsComment/PostNewsCommentAction.class.php[/code]
    mit folgendem Inhalt:
    [code]<?php
    class News_PostNewsCommentAction extends ProjectBaseAction {
    
        function executeWrite(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('News', 'News');
    
            $news_item = null;
            try {
                $news_item = $news_model->getNewsItem($rd->getParameter('news_item'));
            } catch (Exception $e) {
                return "Error";
            }
    
            $comments_model = $this->getContext()->getModel('Comments', 'Extras');
    
            $session = $this->getContext()->getUser();
            $user = null;
    
            if ($session->isAuthenticated()) {
                $user = array(
                    'name' => $session->getAttribute('name'),
                    'id' => $session->getAttribute('id')
                );
            } else {
                $user = array(
                    'name' => 'Gast'
                );
            }
    
            try {
                $comments_model->postCommentAsUser(array(
                    'body' => $rd->getParameter('body'),
                    'user' => $user,
                    'entity' => array(
                        'id' => $news_item['id'],
                        'type' => 'News',
                    )
                ));
            } catch (Exception $e) {
                return "Error";
            }
            return "Success";
        }
    
    }[/code]
    
    Weil in Agavi alles validiert wird, brauchen wir auch noch eine PostNewsComment.validate.xml:
    [code]$ vim app/modules/News/impl/PostNewsComment/PostNewsComment.validate.xml[/code]
    mit dem Inhalt:
    [code]<?xml version="1.0" encoding="UTF-8"?>
    <ae:configurations xmlns="http://agavi.org/agavi/config/parts/validators/1.0" xmlns:ae="http://agavi.org/agavi/config/global/envelope/1.0" parent="%core.config_dir%/validators.xml" >
        <ae:configuration>
            <validators>
                <validator class="number">
                    <argument>news_item</argument>
                </validator>
                <validator class="string">
                    <argument>body</argument>
                </validator>
            </validators>
        </ae:configuration>
    </ae:configurations>[/code]
    
    [h2]CommentsModel um postCommentAsUser erweitern[/h2]
    
    Füge neue Funktion hinzu:
    [code]    public function postCommentAsUser($options) {
            $body = $options['body'];
            $entity = $options['entity'];
            $user = $options['user'];
    
            $comment_class = $entity['type'] . 'Comment';
            $comment = new $comment_class();
    
            $comment->entity_id = $entity['id'];
            $comment->body = $body;
            $comment->user_name = $user['name'];
            $comment->created_at = date('Y-m-d H:i:s', time());
    
            if (isset($user['id'])) {
                $comment->user_id = $user['id'];
            }
    
            $comment->save();
    
        }[/code]
    [h2]Helfermethode für das Erstellen eines Kommentars[/h2]
    Wir haben bis jetzt noch recht viel Logik in der PostNewsCommentAction, um rauszufinden, ob der User eingeloggt ist und das Array für die postComment Funktion zusammen zu bauen.
    
    Deswegen erweitern wir das CommentsModel um folgende Methode:
    [code]    public function postComment($entity_type, $entity_id, $body) {
            $session = $this->getContext()->getUser();
            $user = null;
    
            if ($session->isAuthenticated()) {
                $user = array(
                    'name' => $session->getAttribute('name'),
                    'id' => $session->getAttribute('id')
                );
            } else {
                $user = array(
                    'name' => 'Gast'
                );
            }
    
            $this->postCommentAsUser(array(
                'body' => $body,
                'user' => $user,
                'entity' => array(
                    'id' => $entity_id,
                    'type' => $entity_type,
                )
            ));
        }[/code]
    
    Diese benutzen wir dann gleich in der PostNewsCommentAction und verschlankern diese dann auf den folgenden Code:
    [code]<?php
    class News_PostNewsCommentAction extends ProjectBaseAction {
    
        function executeWrite(AgaviRequestDataHolder $rd) {
            $news_model = $this->getContext()->getModel('News', 'News');
    
            $news_item = null;
            try {
                $news_item = $news_model->getNewsItem($rd->getParameter('news_item'));
            } catch (Exception $e) {
                return "Error";
            }
    
            $comments_model = $this->getContext()->getModel('Comments', 'Extras');
    
            try {
                $comments_model->postComment('News', $news_item['id'], $rd->getParameter('body'));
            } catch (Exception $e) {
                return "Error";
            }
    
            return "Success";
        }
    
    }[/code]
