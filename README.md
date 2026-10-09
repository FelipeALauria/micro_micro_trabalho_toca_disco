# 🎵 Micro Micro: Toca-Discos com Arduino

Um mini toca-discos feito com Arduino. A ideia é simples: você coloca um "disquinho" (um mini disco falso, impresso em 3D) sobre o aparelho, ele é identificado por **NFC**, o disco começa a **girar** e a música correspondente toca por um alto-falante embutido.

Cada disco tem uma tag NFC própria, então cada um representa uma música diferente. A proposta é ter um dispositivo pequeno, bonito e autônomo, alimentado por uma bateria recarregável via USB.

---

## 💡 A ideia

- **Identificação do disco:** uma tag NFC em cada disco diz ao Arduino qual música tocar.
- **Rotação:** um pequeno motor DC faz o disco girar enquanto a música toca, como num toca-discos de verdade.
- **Áudio:** as músicas ficam armazenadas em um cartão SD e saem por um alto-falante dentro do próprio aparelho.
- **Estrutura:** toda a estrutura (carcaça, base, prato e discos) é modelada no FreeCAD e impressa em 3D.

---

## 📁 O que tem neste repositório

```
.
├── firmware/      # Código C++ do Arduino (leitura NFC, controle do motor, áudio)
├── cad/           # Arquivos do FreeCAD (.FCStd), exportações para impressão (.stl) e imagens dos modelos
├── docs/          # Esquemas de ligação, fotos, anotações e referências
├── musicas/       # Músicas que vão para o cartão SD
├── .github/       # Workflows do GitHub Actions (verificação de compilação)
├── INSTALACAO.md  # Passo a passo das ferramentas e bibliotecas necessárias
└── README.md
```

> A estrutura pode mudar conforme o projeto avança. Se criar uma pasta nova, atualize esta seção.

---

## 🛠️ Ferramentas

Para preparar o ambiente (Arduino, bibliotecas, FreeCAD, fatiador e Git), siga o **[INSTALACAO.md](INSTALACAO.md)**.

### Modelagem 3D: use o FreeCAD

Para modelar as peças da impressora 3D (carcaça, prato, discos, suportes), **recomendamos usar o [FreeCAD](https://www.freecad.org/)**:

- É **gratuito e open source**, roda em Windows, Linux e macOS.
- É **paramétrico**: dá para mudar uma medida (ex.: diâmetro do disco ou do eixo do motor) e o modelo inteiro se ajusta.
- O arquivo `.FCStd` pode ser versionado no Git junto com o código, e todo mundo usa a mesma ferramenta.
- Exporta direto para `.stl`, que é o formato que o fatiador da impressora usa.

**Convenção para a pasta `cad/`:**

- Salve sempre o arquivo-fonte **`.FCStd`** (é ele que se edita).
- Exporte o **`.stl`** de cada peça pronta para imprimir.
- Se possível, adicione uma **imagem (`.png`)** da peça para facilitar a visualização no GitHub.
- Use nomes claros: `carcaca.FCStd`, `carcaca.stl`, `disco.FCStd`...

> Dica: arquivos `.FCStd` são binários, então o Git não consegue mesclar alterações de duas pessoas na mesma peça. **Combine quem está editando cada peça** para não perder trabalho.

---

## 🙈 `.gitignore` e `.gitattributes`

Esses dois arquivos ficam na raiz do repositório e controlam **como o Git trata os arquivos do projeto**. Quase nunca é preciso mexer neles, mas é bom saber o que fazem.

### `.gitignore`: o que o Git ignora

Tudo que estiver listado aqui **não vai para o repositório**, mesmo com `git add .`. Serve para não subir arquivos que são lixo, gerados automaticamente ou que só fazem sentido no computador de cada um.

| Regra | O que é | Por que ignorar |
|---|---|---|
| `*.FCBak`, `*.FCStd1` | Backups automáticos que o FreeCAD cria ao salvar | São cópias do `.FCStd`; o histórico do Git já faz esse papel |
| `*.gcode` | Arquivo gerado pelo fatiador para a impressora | É específico de cada impressora/configuração; dá para gerar de novo a partir do `.stl` |
| `build/` | Saída da compilação do Arduino | É gerada toda vez que compila; o código-fonte é o que importa |
| `Thumbs.db`, `desktop.ini`, `.DS_Store` | Arquivos que o Windows e o macOS criam sozinhos nas pastas | Não têm nada a ver com o projeto |
| `.vscode/*` | Configurações pessoais do VS Code | Cada um tem as suas preferências |
| `!.vscode/settings.json`, `!.vscode/extensions.json`, `!.vscode/c_cpp_properties.json` | Exceções (o `!` significa "**não** ignore este") | São configurações do projeto que todos devem ter: `.ino` reconhecido como C++, extensões recomendadas e onde estão as bibliotecas do Arduino |

> Apareceu um arquivo que não deveria subir? Adicione o nome ou o padrão (ex.: `*.log`) no `.gitignore`. Se o arquivo **já foi commitado**, ele continua no repositório até ser removido com `git rm --cached nome-do-arquivo`.

### `.gitattributes`: como o Git trata cada tipo de arquivo

| Regra | O que faz | Por que |
|---|---|---|
| `* text=auto` | Padroniza as quebras de linha dos arquivos de texto | Windows usa `CRLF` e Linux/macOS usam `LF`. Sem isso, o Git pode achar que o arquivo inteiro mudou só porque foi salvo em outro sistema, gerando conflitos falsos |
| `*.FCStd binary` | Marca o arquivo do FreeCAD como binário | Ele é um arquivo compactado. O Git não consegue mostrar diferenças nem mesclar duas versões, então é melhor ele nem tentar |
| `*.stl binary` | Marca os modelos de impressão como binários | Mesmo motivo: arquivos gerados, não editáveis como texto |
| `*.mp3`, `*.wav` `binary` | Marca as músicas como binárias | Áudio não tem "linhas" para comparar |
| `*.jpg`, `*.png` `binary` | Marca as imagens como binárias | Evita que o Git tente converter quebras de linha e corrompa a imagem |

> **Consequência prática:** como arquivos binários não podem ser mesclados, se duas pessoas editarem o mesmo `.FCStd` ao mesmo tempo, uma das versões vai ser perdida no merge. **Combinem antes quem está mexendo em cada peça.**

---

## ⚙️ GitHub Actions (`.github/`)

O **GitHub Actions** é um serviço do GitHub que roda tarefas automaticamente quando algo acontece no repositório (um push, um Pull Request...). Aqui ele é usado para **verificar se o firmware compila** antes de qualquer código entrar na `develop` ou na `master`.

```
.github/
└── workflows/
    └── compilar.yml   # Verificação de compilação do firmware
```

O GitHub só procura workflows dentro de `.github/workflows/`, então **o nome e o lugar dessa pasta não podem mudar**.

### O que o `compilar.yml` faz

1. **Quando roda:** em todo push e todo Pull Request para a `develop` ou a `master`. Também dá para rodar na mão pela aba **Actions → Compilar firmware → Run workflow**.
2. **Onde roda:** o GitHub cria uma máquina Linux temporária, que é apagada no final.
3. **O que faz:**
   - baixa o código do repositório;
   - instala o compilador do Arduino e as bibliotecas listadas no arquivo;
   - compila o `firmware/firmware.ino` para o **Arduino Duemilanove (ATmega328)**.
4. **Resultado:** ✅ se compilou, ❌ se deu erro. O resultado aparece no Pull Request e na aba **Actions**, onde dá para ver o log com a mensagem de erro.

### Por que isso é necessário

- **Pega erro antes de chegar na `develop`:** erro de sintaxe, biblioteca faltando ou nome errado aparecem no PR, e não no computador de quem baixar depois.
- **Avisa se o código não cabe na placa:** o Duemilanove tem só 32 KB de memória para o programa e 2 KB de RAM. Com SD, áudio e NFC juntos, isso estoura fácil, e a compilação falha.
- **Trava a `master` de verdade:** a proteção da `master` está configurada para **só aceitar PR com o check `compilar` ✅**. Sem o workflow, a regra "só entra código validado" dependeria de cada um lembrar de testar.

### O que ele **não** faz

Ele só garante que o código **compila**. Não testa se o NFC lê a tag, se o motor gira ou se o som sai. Isso continua sendo testado com a placa montada, na `develop`.

### Quando mexer nele

- **Adicionou uma biblioteca nova no código?** Adicione também na lista `libraries:` do `compilar.yml`. Senão o check falha com "No such file or directory".
- **Trocou de placa?** Mude o `fqbn:`.
- **Renomeou a pasta ou o arquivo do firmware?** A pasta e o `.ino` precisam ter o **mesmo nome** (`firmware/firmware.ino`). Isso é exigência do Arduino.

---

## 🧩 Configurações do VS Code (`.vscode/`)

A pasta `.vscode/` guarda configurações que o VS Code aplica automaticamente quando alguém abre o projeto. Assim **todo mundo trabalha com o mesmo ambiente** sem precisar configurar nada na mão.

```
.vscode/
├── settings.json           # Configurações do editor para este projeto
├── extensions.json         # Extensões recomendadas
└── c_cpp_properties.json   # Onde ficam as bibliotecas do Arduino (para o autocompletar)
```

| Arquivo | O que faz | Por que é necessário |
|---|---|---|
| `settings.json` | Diz ao VS Code para tratar arquivos `.ino` como **C++** | Por padrão o VS Code não conhece `.ino`. Sem isso, o código fica sem cores, sem autocompletar e sem detecção de erros |
| `extensions.json` | Recomenda as extensões **C/C++** (Microsoft) e **Arduino Community Edition** | Ao abrir o projeto, o VS Code pergunta se quer instalar. Todo mundo fica com as mesmas ferramentas |
| `c_cpp_properties.json` | Aponta onde estão os arquivos do Arduino instalados pela Arduino IDE (no Windows e no Linux) e define a placa (ATmega328, 16 MHz) | Sem isso, o VS Code não acha o `Arduino.h` e marca `Serial`, `pinMode`, `digitalWrite` etc. como erro (sublinhado vermelho), mesmo com o código certo |

> **Importante:** o `c_cpp_properties.json` só funciona se a **Arduino IDE estiver instalada** e tiver sido aberta pelo menos uma vez com a placa Duemilanove selecionada (veja o [INSTALACAO.md](INSTALACAO.md)). Se o sublinhado vermelho continuar, rode **Ctrl+Shift+P → "Reload Window"**.

> Essas configurações são **só do editor**: não afetam a compilação. Quem usa só a Arduino IDE pode ignorar a pasta `.vscode/`. Configurações pessoais (tema, fonte...) devem ficar nas configurações do usuário do VS Code, não aqui. Por isso o `.gitignore` só deixa subir esses três arquivos.

---

## 🌿 Branches

O projeto segue um fluxo simples com duas branches principais:

| Branch | Para que serve |
|---|---|
| **`master`** | A branch principal. Aqui fica apenas o código **refinado e validado**, ou seja, o que já foi testado e funciona. Ninguém faz commit direto nela. |
| **`develop`** | A branch de integração, antes da `master`. É onde as partes de cada um se juntam para **testar com os outros componentes** (NFC + motor + áudio + carcaça funcionando juntos). |

### Como trabalhar (importante!)

Como somos três pessoas mexendo no mesmo projeto, **cada um cria sua própria branch a partir da `develop`**. Assim evitamos conflitos e ninguém sobrescreve o trabalho do outro.

```bash
# 1. Atualize a develop
git checkout develop
git pull origin develop

# 2. Crie sua branch a partir dela
git checkout -b nome-da-sua-branch   # ex: feature/leitor-nfc, feature/motor, cad/carcaca

# 3. Trabalhe, faça commits e envie
git add .
git commit -m "Descrição clara do que foi feito"
git push origin nome-da-sua-branch
```

Depois, abra um **Pull Request da sua branch para a `develop`**. Quando tudo estiver testado e funcionando na `develop`, ela é mesclada na `master`.

**Fluxo resumido:**

```
sua-branch  ──►  develop  ──►  master
 (trabalho)     (testes)     (versão final)
```

**Boas práticas:**

- Antes de abrir o PR, faça `git pull origin develop` na sua branch para trazer as últimas mudanças e resolver conflitos do seu lado.
- Use nomes de branch que digam o que está sendo feito.
- Commits pequenos e com mensagens claras.
- Nunca faça commit direto na `master`.

---

## 👥 Equipe

- Bruna Fontes de Castro
- Felipe do Amparo Lauria
- João Carlos Gonçalves Nobre
