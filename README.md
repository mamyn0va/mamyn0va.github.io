# REX Go Boris & Julien

Un retour d'expérience **honnête** sur Golang, par des dévs PHP (et JS)

<img src="https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/05/1495193438gophp-1024x518.png" style="width: 80%" />

---

## 🤪 pourquoi Go ? WTF?!

<p style="color:#777777;font-style:italic">« Go, ça scale à l'infini, y a pas d'empreinte mémoire »<span style="font-style:none"> - J. Laforge</span></p>

--

<p style="color:#777777;font-style:italic">« Pas besoin de tuner ton pool 🐔 de threads... »<span style="color:#777777"> - J. Laforge</span></p>

--

<p style="color:#777777;font-style:italic">« Go, c'est bien. »<span style="color:#777777"> - Anne Onyme</span></p>

???
Some note.

---

## 🦸changement de paradigme

- on peut faire de l'**objet** (c'est pas obligé), mais les concepts sont plus simples (il n'y pas d'héritage, mais de la composition et des interfaces)

--

- **asynchronisme** : c'est ultra simple (et pour le coup très élégant) même si on s'est pris les pieds dans le tapis (et il y a beaucoup de tapis)
 - goroutines
 - waitGroups
 - channels

--

- **concurrence** :
 - *mutex* : on l'a utilisé une fois dans un mock, mais sinon on n'a jamais eu à gérer de la concurrence

 - ⚠️  boucles `for` qui lancent des goroutines ! ça crée de la concurrence là où il n'y en a pas !

---

🤔 accès concurrent non protégé à une variable (*race condition*)

```
for _, num := range []int{1, 2, 3} {
	go func() {
		fmt.Println(num)
	}()
}
```

--

```shell
$ go run ./...
3 
3 
3
```

--

😀 solution : cloner la variable (ou la passer en param de la closure, par copie)
```golang
for _, num := range []int{1, 2, 3} {
*   num := num
	go func() {
		fmt.Println(num)
	}()
}
```

```shell
$ go run ./...
1
3
2
```

---

## ⚠️  nos points durs

![](https://media.giphy.com/media/mIvrv5Qe0kHlu/giphy.gif)

--

- `defer` => LIFO

--

- *unmarshaling* : ne plante que si le format est incorrect, comment gérer les `required` ? 
	- 💡 utiliser des pointeurs ou une librairie ([go-validator/validator](https://github.com/go-validator/validator), [gin-gonic](https://github.com/gin-gonic/gin))

--

- `string` / `[]byte` / `bytes.Buffer` WTF!? on fait beaucoup de conversions

--

- `io.ReadCloser` == flux, c'est du READ_ONCE, ensuite il faut "rembobiner" !

--

- googling (*"golang struct"* pas *"go struct"*)

---

## 🏎️  injection de dépendances

- l'injection se base sur les **interfaces**, principe élégant et puissant, mais...

--

- ...certaines librairies ne proposent pas d'interface (ex: [mgo](https://github.com/globalsign/mgo)) : il faut abstraire la librairie (dans une interface) pour pouvoir l'injecter

--

- injection par **conteneur** (mode sport turbo diesel) aka *"DIC"*

--

- injection manuelle : constructeur / setters / fonctions
 - ça complexifie rapidement les signatures de fonctions

 - il faut initialiser toutes les dépendances, et dans le bon ordre !

 - 🕮  à creuser : [google/wire](https://github.com/google/wire)

---

👉 exemple d'interface pour un mapper de BD

```
type Mapper struct {
    Db     core.ISamDatabase
    Logger logger.ISamLogger
}

type IMapper interface {
	GetById(id string) (*model.Resource, error)
	GetAll(limit int, offset int) (model.Resources, error)
	Store(resource model.Resource) error
	DeleteById(id string) error
	Update(resource model.Resource) error
}

func NewMapper(db core.ISamDatabase, logger logger.ISamLogger) IMapper {
    return &Mapper{Db: db, Logger: logger}
}

func (mapper *Mapper) GetById(id string) (*model.Resource, error) {
	...
}
```

---

## les tests

- uniquement des vrais tests unitaires

- tests fonctionnels pour IVA

- mocking manuel ou automatique avec [gomock](https://github.com/golang/mock) ou [testify](https://github.com/stretchr/testify)
 - `assert.Equal` : attention aux pointeurs!

- travail fastidieux, mais outillage similaire à PHP/Java (hormis la génération des mocks)

---

```
func TestService_GetById(t *testing.T) {
	mockCtrl := gomock.NewController(t)
	defer mockCtrl.Finish()

	id := "id"

	wantResource := model.Resource{}

	mapper := resourcemappermock.NewMockIMapper(mockCtrl)
	mapper.EXPECT().GetById(id).Times(1).Return(&wantResource, nil)

	logger := &mocks.MockSamLogger{}

	service := NewService(mapper, logger)
	gotResource, err := service.GetById(id)

	assert.NoError(t, err)
	assert.Equal(t, &wantResource, gotResource)
}
```

---

## 👮 organisation du code

- `go.mod` (exit `go dep`)

--

- pas de vendoring 🤢

--

- en Go, il n'y a pas de classe, l'unité est le package, chaque package doit être dans un dossier spécifique (éventuellement splité en plusieurs fichiers)

--

- choix de notre arborescence de code = OOP (main > controller > business > mapper (aka DAO) / enabler > model (aka DTO)

--

- après plusieurs mois d'expérience, est-ce bien le bon choix ? pourrait-on être plus efficace en étant plus pragmatique dans le code ? nos vieux réflexes OOP ont la vie dure !

---

## 🛠️  notre toolkit pour construire une API Web

- conf : [spf13/viper](https://github.com/spf13/viper)

- http router : [gin-gonic/gin](https://github.com/gin-gonic/gin)

- mongo driver : [globalsign/mgo](https://github.com/globalsign/mgo)

- logger : [uber-go/zap](https://github.com/uber-go/zap)

- UUID : [gofrs/uuid](https://github.com/gofrs/uuid)

- JWT : [dgrijalva/jwt-go](https://github.com/dgrijalva/jwt-go)

- YAML parser : [go-yaml/yaml](https://github.com/go-yaml/yaml)

- numbers & arithmetic : [shopspring/decimal](https://github.com/shopspring/decimal)

- mocking : [golang/mock](https://github.com/golang/mock)

- unit tests assertions : [stretchr/testify](https://github.com/stretchr/testify)

- file watcher : [cortesi/modd](https://github.com/cortesi/modd)

- validation de la conf : [Grokzen/pykwalify](https://github.com/Grokzen/pykwalify)

---

## 📝 quel éditeur?

--

### 🤔 quels sont les besoins ?

--

- ✨ coloration syntaxique / formatage auto / linting

--

- 🧙 naviguer dans le code / refactorer

--

- 🐜 débogage

--

- 🧬une vue outline (structure du code)

--

- 🗂 une vue projet (arborescence)

---

## 📝 quel éditeur?

### alternatives

- Atom + go-plus + ide-go

- (Space)Vim + vim-go

- VSCode + extension go

- Intellij Ultimate + go plugin

Les 4 éditeurs proposent toutes ces fonctionnalités, mais le seul qui fonctionne out-of-the-box, c'est Vim.

Les 3 autres sont très bien aussi, mais soit la conf est plus complexe, soit il y a des bugs.

Le problème avec Vim, c'est que pour être efficace, il faut connaître tous les raccourcis clavier, et ça prend énormément de temps.

Au final, avec un peu de persévérance, on a réussi à configurer Intellij pour pouvoir builder, lancer et tester notre app.

Les deux gros atouts d'Intellij, c'est le debugger et le refactoring qui sont juste indispensables. On gagne un temps fou.

---

## notre ressenti

très rapide : 

394 tests en moins de 4s, et comme c'est caché, la plupart du temps ça prend moins d'une seconde

build en moins de 2 secondes

démarre quasi-instantanément

taille du binaire principal : 20 Mo

encore un peu jeune sur certains points :

gestion des dépendances

driver officiel pour mongo (vient tout juste de sortir)

communauté moins présente que pour PHP (SOF, github, ...)

certaines librairies dangereuses (satori/go.uuid, rand/math, ...)

très bien adapté au contexte cloud (juste un binaire + un fichier de conf dans un container et zou !)

windows :(

pas nécessairement besoin de framework ou d'ORM, le langage est très riche et très puissant

---

<p style="font-size:72px">
<br/>
<br/>
&nbsp;&nbsp;🦙 des questions?
</p>
