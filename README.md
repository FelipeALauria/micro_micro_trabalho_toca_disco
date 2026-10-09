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
