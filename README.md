# Roteiro de Aula — AppRevisao (Android Studio)

> **Tempo total estimado: ~50 minutos**  
> Cada seção mostra o código real do projeto e explica o que cada parte faz.

---

## O que o app faz

Uma tela simples com:
- Um **texto** escrito "Olá Mundo"
- Um **botão** escrito "Iniciar Cadastro"

Quando o aluno clica no botão, aparece uma **mensagem rápida** (chamada Toast) na parte de baixo da tela, dizendo "Seja bem vindo(a) !", e some sozinha depois de 2 segundos.

---

## Antes de começar: o que é Android Studio?

O **Android Studio** é o programa (IDE) oficial do Google para criar apps Android. Dentro dele você vai encontrar:
- **Editor de código** → onde escrevemos Java e XML
- **Design view** → uma pré-visualização da tela do celular (arrasta e solta componentes)
- **Emulador** → um celular virtual que roda no computador para testar o app
- **Árvore do projeto** → painel lateral esquerdo que mostra todas as pastas e arquivos

---

## PARTE 1 — Estrutura do Projeto (5 min)

> **O que mostrar:** No Android Studio, abra o painel lateral esquerdo (Project) e mostre as pastas.

Todo projeto Android segue esta organização:

```
app/src/main/
├── AndroidManifest.xml          ← "RG" do app (documento de identidade)
├── java/.../MainActivity.java   ← LÓGICA (o que o app FAZ)
└── res/                         ← RECURSOS (o que o usuário VÊ)
    ├── layout/activity_main.xml ← Desenho da tela
    └── values/
        ├── strings.xml          ← Todos os textos do app
        ├── colors.xml           ← Cores
        └── themes.xml           ← Tema visual (Material Design)
```

### Conceito importante: XML vs Java

| | XML | Java |
|---|---|---|
| **O que é?** | Linguagem de marcação (como HTML) | Linguagem de programação |
| **Para que serve?** | Define a **aparência** da tela | Define o **comportamento** (lógica) |
| **Exemplo** | "Coloque um botão aqui" | "Quando clicar no botão, faça isso" |
| **Analogia** | É o **corpo** do app | É o **cérebro** do app |

---

## PARTE 2 — AndroidManifest.xml — o "RG" do app (5 min)

> **O que mostrar:** Abra o arquivo `app/src/main/AndroidManifest.xml` no Android Studio.

O **AndroidManifest.xml** é o primeiro arquivo que o Android lê quando abre seu app. É como o RG: contém todas as informações de identidade.

### Código completo:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="br.com.etecmorato.apprevisao">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.AppRevisao">

        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
</manifest>
```

### Explicação linha por linha:

| Trecho | O que é | Explicação |
|--------|---------|------------|
| `package="br.com.etecmorato.apprevisao"` | **Package** | É o "CPF" do app. Identificador único. Nenhum outro app no mundo pode ter o mesmo. |
| `android:icon="@mipmap/ic_launcher"` | **Ícone** | A imagem que aparece na tela do celular (aquele quadradinho do app). `@mipmap/` aponta para a pasta de ícones. |
| `android:label="@string/app_name"` | **Nome** | O nome que aparece embaixo do ícone no celular. O `@string/` puxa o texto do arquivo `strings.xml`. |
| `android:theme="@style/Theme.AppRevisao"` | **Tema** | As cores e o visual do app (Material Design). |
| `<activity android:name=".MainActivity">` | **Activity** | Registra uma **tela** do app. Cada tela precisa ser registrada aqui. O `.MainActivity` aponta para a classe Java. |
| `android:exported="true"` | **Exported** | Permite que o sistema Android abra esta tela (precisa ser true na tela principal). |
| `<action android:name="android.intent.action.MAIN" />` | **MAIN** | Diz que esta é a ação principal do app. |
| `<category android:name="android.intent.category.LAUNCHER" />` | **LAUNCHER** | Diz que esta tela deve aparecer na lista de apps do celular. **Sem isso, o app não aparece!** |

### Resumindo para os alunos:
> "O AndroidManifest é como preencher uma ficha: você diz o nome do app, qual ícone usar, e qual tela abre primeiro. Sem esse arquivo, o Android nem sabe que o app existe."

---

## PARTE 3 — strings.xml — textos centralizados (5 min)

> **O que mostrar:** Abra o arquivo `app/src/main/res/values/strings.xml`.

### Código completo:

```xml
<resources>
    <string name="app_name">AppRevisao</string>
    <string name="lblBoasVidas">Olá Mundo</string>
    <string name="btnIniciar">Iniciar Cadastro</string>
</resources>
```

### O que cada linha faz:

| Linha | `name` (apelido) | Texto que aparece | Onde é usado |
|-------|-------------------|-------------------|--------------|
| `app_name` | Nome do app | "AppRevisao" | No AndroidManifest (embaixo do ícone) |
| `lblBoasVidas` | Texto de boas vindas | "Olá Mundo" | No TextView da tela |
| `btnIniciar` | Texto do botão | "Iniciar Cadastro" | No Button da tela |

### Por que usar strings.xml em vez de escrever o texto direto?

**Errado** (hardcoded — texto direto no layout):
```xml
<Button android:text="Iniciar Cadastro" />
```

**Certo** (referência ao strings.xml):
```xml
<Button android:text="@string/btnIniciar" />
```

**Vantagens:**
1. **Tradução fácil**: basta criar uma pasta `values-en/strings.xml` com os textos em inglês, e o Android troca automaticamente conforme o idioma do celular
2. **Manutenção**: se o texto "Iniciar Cadastro" aparecer em 10 telas, você muda em **um lugar só**
3. **Organização**: todos os textos ficam em um arquivo só

### Resumindo para os alunos:
> "O strings.xml é como uma lista telefônica de textos. Você dá um apelido (name) para cada texto, e em qualquer lugar do app usa esse apelido com @string/. Se precisar mudar o texto, muda em um lugar só."

---

## PARTE 4 — activity_main.xml — o layout da tela (10 min)

> **O que mostrar:** Abra `app/src/main/res/layout/activity_main.xml`. Mostre primeiro o **Design view** (aba de pré-visualização) e depois troque para o **Code view** (código).

### O que o aluno vai ver na tela:

```
┌─────────────────────────────┐
│                             │
│         Olá Mundo           │  ← TextView
│                             │
│                             │
│    ┌───────────────────┐    │
│    │ Iniciar Cadastro  │    │  ← Button
│    └───────────────────┘    │
│                             │
│                             │
└─────────────────────────────┘
```

### Código completo:

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:text="@string/lblBoasVidas"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btnXMLIniciarCadastro"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/btnIniciar"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView"
        app:layout_constraintVertical_bias="0.454" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

### Explicação de cada parte:

#### 1. ConstraintLayout (o "container" da tela)
```xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">
```
- É o **layout principal** — um "container" que segura todos os componentes
- `match_parent` = ocupa **TODA** a largura e altura da tela
- Funciona com **constraints** (restrições): você "amarra" cada componente a outro ou às bordas da tela
- **Analogia:** Imagine um quadro de cortiça. Você usa alfinetes (constraints) para prender papéis (componentes) no quadro.

#### 2. TextView (o texto "Olá Mundo")
```xml
<TextView
    android:id="@+id/textView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginTop="16dp"
    android:text="@string/lblBoasVidas"
    app:layout_constraintLeft_toLeftOf="parent"
    app:layout_constraintRight_toRightOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
```

| Atributo | Valor | O que faz |
|----------|-------|-----------|
| `android:id` | `@+id/textView` | Dá um **nome** ao componente. O `+` significa "criar um id novo". |
| `layout_width` | `wrap_content` | A largura se **ajusta ao texto** (não ocupa a tela toda). |
| `layout_height` | `wrap_content` | A altura se **ajusta ao texto**. |
| `layout_marginTop` | `16dp` | Espaçamento de 16dp acima. **dp** = unidade de medida do Android (funciona igual em telas de tamanhos diferentes). |
| `android:text` | `@string/lblBoasVidas` | Puxa o texto "Olá Mundo" do `strings.xml`. |
| `constraintLeft_toLeftOf="parent"` | — | Amarra a **esquerda** do texto à **esquerda da tela**. |
| `constraintRight_toRightOf="parent"` | — | Amarra a **direita** do texto à **direita da tela**. |
| `constraintTop_toTopOf="parent"` | — | Amarra o **topo** do texto ao **topo da tela**. |

> Quando amarra esquerda E direita ao parent, o componente fica **centralizado horizontalmente**.

#### 3. Button (o botão "Iniciar Cadastro")
```xml
<Button
    android:id="@+id/btnXMLIniciarCadastro"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/btnIniciar"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/textView"
    app:layout_constraintVertical_bias="0.454" />
```

| Atributo | O que faz |
|----------|-----------|
| `android:id="@+id/btnXMLIniciarCadastro"` | Nome do botão no XML. **Este é o nome que o Java vai usar para encontrar o botão!** |
| `android:text="@string/btnIniciar"` | Texto "Iniciar Cadastro" (vem do strings.xml). |
| `constraintBottom_toBottomOf="parent"` | Amarra em baixo à base da tela. |
| `constraintStart/End_toStart/EndOf="parent"` | Amarra esquerda e direita → **centraliza**. |
| `constraintTop_toBottomOf="@+id/textView"` | Amarra o topo do botão **abaixo do TextView**. Ou seja, o botão fica abaixo do texto. |
| `vertical_bias="0.454"` | Desloca o botão levemente para cima (0 = colado no topo, 0.5 = meio, 1 = colado embaixo). |

#### Diferença match_parent vs wrap_content (mostrar no quadro):

```
match_parent:                    wrap_content:
┌──────────────────────────┐     ┌──────────────────────────┐
│ ████████████████████████ │     │     ████████████         │
│ (ocupa TUDO)             │     │     (só o necessário)    │
└──────────────────────────┘     └──────────────────────────┘
```

### Resumindo para os alunos:
> "O XML é como desenhar a tela no papel quadriculado. Cada componente (texto, botão) tem um nome (id), um tamanho e uma posição definida pelas constraints. O ConstraintLayout é como um quadro de cortiça onde você espeta os componentes com alfinetes."

---

## PARTE 5 — MainActivity.java — a lógica ⭐ (15 min)

> **O que mostrar:** Abra `app/src/main/java/br/com/etecmorato/apprevisao/MainActivity.java`.  
> Esta é a parte mais importante. Vá devagar.

### Passo 1: Package (pacote)

```java
package br.com.etecmorato.apprevisao;
```

**O que é:** Define o "endereço" desta classe Java. Todo arquivo Java precisa de um package.  
**Analogia:** É como o CEP de uma carta. Diz onde esta classe mora dentro do projeto.

---

### Passo 2: Imports (importações)

```java
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;
```

**O que é:** Trazem classes prontas do Android para dentro do nosso código.  
**Analogia:** É como ir na biblioteca e pegar livros emprestados. Sem o `import`, o Java não sabe o que é um `Button` ou um `Toast`.

| Import | Classe | Para que serve |
|--------|--------|----------------|
| `AppCompatActivity` | Classe-base de tela | Faz a nossa classe virar uma "tela" do Android |
| `Bundle` | Pacote de dados | Salva/restaura dados quando a tela gira |
| `View` | Componente visual | Classe-mãe de TODOS os componentes (botão, texto, imagem...) |
| `Button` | Botão | Componente de botão clicável |
| `Toast` | Mensagem rápida | Aquela mensagenzinha que aparece e some sozinha |

---

### Passo 3: Declaração da classe

```java
public class MainActivity extends AppCompatActivity {
```

**O que é cada palavra:**
| Palavra | Significado |
|---------|-------------|
| `public` | Qualquer parte do sistema pode acessar esta classe |
| `class` | Estamos criando uma **classe** (molde de objeto) |
| `MainActivity` | **Nome** da nossa classe (e da nossa tela) |
| `extends` | Significa **herança** — estamos herdando tudo de outra classe |
| `AppCompatActivity` | Classe do Android que já sabe ser uma tela. Herdar dela faz a nossa classe virar uma tela também. |

**Analogia:** Imagine que `AppCompatActivity` é um "modelo de tela em branco". Com `extends`, a gente pega esse modelo e personaliza.

---

### Passo 4: Declarar a variável do botão

```java
Button btnJAVAIniciarCadastro;
```

**O que é:** Cria uma variável do tipo `Button`. Ainda está vazia (não aponta para nada).  
**Analogia:** É como escrever um nome numa etiqueta adesiva, mas ainda não colou em nada.

**Convenção do professor:**
- `btnJAVA...` = variável no Java
- `btnXML...` = id lá no layout XML
- Isso ajuda a saber de que "mundo" cada coisa é

---

### Passo 5: Método onCreate() — Ciclo de Vida

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
```

**O que é:** O método `onCreate()` é **automaticamente chamado pelo Android** quando a tela é criada. É o primeiro método que roda.

| Trecho | Significado |
|--------|-------------|
| `@Override` | Estamos **sobrescrevendo** (substituindo) o método da classe-mãe. O Android já tem um `onCreate` padrão; nós estamos colocando o nosso código dentro dele. |
| `protected` | Só esta classe e suas filhas podem usar este método |
| `void` | O método não retorna nenhum valor |
| `Bundle savedInstanceState` | Dados salvos de uma execução anterior (ex.: se girou a tela) |
| `super.onCreate(savedInstanceState)` | **Obrigatório!** Chama o `onCreate` original para o Android fazer suas coisas internas primeiro |

**Analogia:** Quando você liga a TV, ela faz umas configurações internas (super.onCreate) e depois mostra a imagem. O `onCreate` é esse momento de "ligar".

---

### Passo 6: setContentView() — Carregar o layout

```java
setContentView(R.layout.activity_main);
```

**O que é:** Diz ao Android "pegue o arquivo `activity_main.xml` e exiba na tela".

| Trecho | Significado |
|--------|-------------|
| `setContentView()` | "Defina o conteúdo visual" da tela |
| `R` | Classe gerada automaticamente pelo Android. Ela é um "catálogo" que conhece todos os recursos (layouts, strings, ids, imagens). |
| `R.layout` | Dentro do catálogo R, na seção de layouts |
| `activity_main` | O nome do arquivo XML (sem a extensão .xml) |

**Analogia:** `R` é como o Google — ele sabe encontrar qualquer recurso do projeto. Você pede `R.layout.activity_main` e ele encontra o arquivo XML para você.

> **Sem esta linha, a tela fica em branco!**

---

### Passo 7: findViewById() — A ponte Java ↔ XML

```java
btnJAVAIniciarCadastro = findViewById(R.id.btnXMLIniciarCadastro);
```

**O que é:** Procura no layout XML o componente que tem o id `btnXMLIniciarCadastro` e **conecta** ele à variável Java `btnJAVAIniciarCadastro`.

**Veja a conexão:**

```
MUNDO XML (layout)                    MUNDO JAVA (lógica)
┌────────────────────────┐            ┌──────────────────────────┐
│ <Button                │            │                          │
│   android:id=          │            │ Button btnJAVAIniciar... │
│   "@+id/btnXML..."  ◄─┼──PONTE──►──┤ = findViewById(          │
│   />                   │            │     R.id.btnXML...)      │
└────────────────────────┘            └──────────────────────────┘
```

**Sem o `findViewById()`, o Java não consegue "conversar" com o botão do layout.** Seria como ter um controle remoto sem pilha — o botão existe na tela mas ninguém consegue usá-lo.

---

### Passo 8: setOnClickListener() — Evento de clique

```java
btnJAVAIniciarCadastro.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(
                MainActivity.this,
                "Seja bem vindo(a) !",
                Toast.LENGTH_SHORT
        ).show();
    }
});
```

**O que é:** Adiciona um "ouvinte" (listener) ao botão. O Android fica **esperando** o usuário clicar. Quando clica, executa tudo que está dentro do `onClick()`.

**Explicação detalhada:**

| Trecho | O que faz |
|--------|-----------|
| `setOnClickListener(...)` | "Quando alguém clicar neste botão, faça o seguinte..." |
| `new View.OnClickListener()` | Cria um **objeto ouvinte** — alguém que fica de plantão esperando o clique |
| `onClick(View view)` | Método que é executado **no momento do clique** |
| `Toast.makeText(...)` | Cria a mensagem rápida (mas ainda não exibe) |
| `MainActivity.this` | 1º parâmetro: o **contexto** (qual tela está pedindo o Toast) |
| `"Seja bem vindo(a) !"` | 2º parâmetro: o **texto** que vai aparecer |
| `Toast.LENGTH_SHORT` | 3º parâmetro: **duração** (SHORT ≈ 2 segundos, LONG ≈ 3,5 segundos) |
| `.show()` | **Exibe** o Toast na tela. **Se esquecer o `.show()`, nada aparece!** |

**Analogia:** O `OnClickListener` é como uma campainha. Você instala a campainha no botão (`setOnClickListener`). Quando alguém aperta (`onClick`), a campainha toca (executa o código).

### Resumindo para os alunos:
> "O Java faz 4 coisas: (1) declara a variável, (2) carrega a tela, (3) conecta Java ao XML com findViewById, (4) define o que acontece no clique. Pronto, o app funciona!"

---

## PARTE 6 — Executar o app (5 min)

> **O que fazer no Android Studio:**

1. Clique no botão verde **▶ Run** (ou `Shift + F10`)
2. Escolha o emulador (celular virtual) ou conecte um celular via USB
3. Aguarde o app abrir
4. Clique no botão **"Iniciar Cadastro"**
5. Veja o **Toast** "Seja bem vindo(a) !" aparecer embaixo da tela

```
O que aparece no celular:

┌─────────────────────────────┐
│                             │
│         Olá Mundo           │
│                             │
│                             │
│    ┌───────────────────┐    │
│    │ Iniciar Cadastro  │    │  ← Aluno clica aqui
│    └───────────────────┘    │
│                             │
│                             │
│  ┌───────────────────────┐  │
│  │ Seja bem vindo(a) !   │  │  ← Toast aparece aqui
│  └───────────────────────┘  │
└─────────────────────────────┘
```

---

## Diagrama: Fluxo Completo do App

```
[Usuário abre o app no celular]
        │
        ▼
[Android lê o AndroidManifest.xml]
  → "Qual tela tem LAUNCHER?"
        │
        ▼
[Encontra: MainActivity]
  → Chama o método onCreate()
        │
        ▼
[super.onCreate() → configs internas]
        │
        ▼
[setContentView(R.layout.activity_main)]
  → Carrega o XML e desenha a tela
        │
        ▼
[findViewById() → conecta Java ao botão do XML]
        │
        ▼
[setOnClickListener() → instala o "ouvinte"]
        │
        ▼
[Tela pronta! Exibe: "Olá Mundo" + Botão]
        │
        ▼
[Usuário clica no botão]
        │
        ▼
[onClick() é executado]
        │
        ▼
[Toast.makeText().show()]
        │
        ▼
[Mensagem "Seja bem vindo(a) !" aparece por 2 segundos]
```

---

## Conceitos-Chave — Resumão para o Quadro

| Conceito | O que é | Analogia |
|----------|---------|----------|
| **XML** | Linguagem que define a aparência da tela | O **corpo** do app |
| **Java** | Linguagem que define o comportamento/lógica | O **cérebro** do app |
| **Activity** | Uma tela do app | Cada tela = uma Activity |
| **AndroidManifest** | Documento de identidade do app | O **RG** |
| **strings.xml** | Arquivo com todos os textos | **Lista telefônica** de textos |
| **ConstraintLayout** | Container que organiza componentes | **Quadro de cortiça** |
| **TextView** | Componente que exibe texto | **Etiqueta** |
| **Button** | Componente de botão clicável | **Campainha** |
| **match_parent** | Ocupa todo o espaço disponível | Foto **paisagem** na parede |
| **wrap_content** | Ajusta ao tamanho do conteúdo | Foto **3x4** na parede |
| **R** | Catálogo de todos os recursos do projeto | O **Google** do projeto |
| **findViewById()** | Liga o Java ao componente do XML | O **cabo USB** entre os dois mundos |
| **OnClickListener** | Ouvinte que espera o clique | **Campainha** instalada no botão |
| **Toast** | Mensagem rápida e temporária | **Post-it** que desgruda sozinho |
| **onCreate()** | 1º método executado ao abrir a tela | **Ligar a TV** |

---

## Exercícios Práticos para os Alunos

### Nível Fácil
1. **Trocar o texto do Toast**: Vá no `MainActivity.java`, ache `"Seja bem vindo(a) !"` e troque pelo nome do aluno
2. **Mudar o texto do botão**: Vá no `strings.xml`, ache `btnIniciar` e troque o valor
3. **Trocar o texto de boas vindas**: No `strings.xml`, troque `"Olá Mundo"` por outra frase

### Nível Médio
4. **Adicionar um segundo botão**: No XML, copie o `<Button>`, mude o `android:id` e o texto. No Java, declare outra variável, faça `findViewById` e crie outro `setOnClickListener`
5. **Adicionar um EditText** (campo de digitação): No XML adicione `<EditText>`, no Java capture o texto com `editText.getText().toString()` e mostre no Toast
6. **Trocar as cores**: Em `res/values/themes.xml`, mude o `colorPrimary`

### Nível Avançado (desafio para casa)
7. **Criar uma segunda tela**: Crie outra Activity e navegue até ela usando `Intent`
8. **Passar dados entre telas**: Use `putExtra()` para enviar o nome digitado para a segunda tela

---

## Perguntas para Revisão (pode usar como quiz)

1. Qual a diferença entre `match_parent` e `wrap_content`?
2. Para que serve o `findViewById()`?
3. O que é um `Toast` e para que ele serve?
4. Por que usamos o arquivo `strings.xml` em vez de escrever o texto direto no layout?
5. O que acontece quando o método `onCreate()` é chamado?
6. Qual arquivo o Android lê primeiro para saber qual tela abrir?
7. O que acontece se você esquecer o `.show()` no Toast?
8. O que o `setOnClickListener` faz?

---

## Gabarito das Respostas

1. `match_parent` ocupa todo o espaço disponível; `wrap_content` ajusta ao tamanho do conteúdo
2. Conecta um objeto Java a um componente visual do XML (procura pelo id)
3. Mensagem rápida e temporária que aparece na tela e some sozinha (feedback ao usuário)
4. Para centralizar textos em um lugar só, facilitar tradução e manutenção
5. O layout é carregado (`setContentView`), variáveis são vinculadas (`findViewById`) e listeners são configurados
6. O `AndroidManifest.xml` — ele identifica a Activity marcada com `LAUNCHER`
7. O Toast é criado mas **não aparece na tela** (o `.show()` é obrigatório)
8. Instala um "ouvinte" no botão que fica esperando o clique do usuário para executar o código
