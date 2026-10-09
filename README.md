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
├── audio/         # Músicas/arquivos de exemplo para o cartão SD (se aplicável)
└── README.md
```

> A estrutura pode mudar conforme o projeto avança. Se criar uma pasta nova, atualize esta seção.

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