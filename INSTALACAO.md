# 🧰 Instalação e configuração do ambiente

Este guia lista tudo o que cada pessoa precisa instalar para trabalhar no projeto **Micro Micro**.

> **Sobre o `gcc`:** não é preciso instalar o `gcc` comum. O Arduino usa um compilador próprio, o **`avr-gcc`**, que já vem junto com a Arduino IDE (ou com o `arduino-cli`). Instalando um deles, a compilação do código C++ já funciona.

---

## 1. Git

Para clonar o repositório e trabalhar com as branches.

| Sistema | Como instalar |
|---|---|
| Windows | Baixe em [git-scm.com](https://git-scm.com/download/win) |
| Linux (Debian/Ubuntu) | `sudo apt install git` |
| macOS | `brew install git` ou `xcode-select --install` |

Configure seu nome e e-mail (só na primeira vez):

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
```

Clone o projeto e entre na `develop`:

```bash
git clone <URL-DO-REPOSITORIO>
cd <pasta-do-projeto>
git checkout develop
```

---

## 2. Arduino (compilar e enviar o código)

Escolha **uma** das opções abaixo.

### Opção A: Arduino IDE (mais simples)

1. Baixe e instale em [arduino.cc/en/software](https://www.arduino.cc/en/software).
2. Em **Ferramentas → Placa**, selecione **Arduino Duemilanove or Diecimila**.
3. Em **Ferramentas → Processador**, selecione **ATmega328P** (ou ATmega168, conforme o chip da placa).
4. Em **Ferramentas → Porta**, selecione a porta USB onde o Arduino está conectado.

### Opção B: `arduino-cli` (linha de comando)

Instalação: [arduino.github.io/arduino-cli](https://arduino.github.io/arduino-cli/latest/installation/)

```bash
# Instala o núcleo das placas AVR (inclui o avr-gcc)
arduino-cli core update-index
arduino-cli core install arduino:avr

# Compilar (a partir da raiz do repositório)
arduino-cli compile --fqbn arduino:avr:diecimila:cpu=atmega328 firmware/

# Enviar para a placa (troque a porta: COM3 no Windows, /dev/ttyUSB0 no Linux)
arduino-cli upload -p /dev/ttyUSB0 --fqbn arduino:avr:diecimila:cpu=atmega328 firmware/
```

### Driver USB

O Duemilanove usa o chip **FTDI** para a comunicação USB. No Linux e macOS ele normalmente funciona direto. No Windows, se a placa não aparecer em "Porta", instale o driver em [ftdichip.com/drivers/vcp-drivers](https://ftdichip.com/drivers/vcp-drivers/).

> **Linux:** se der erro de permissão na porta, adicione seu usuário ao grupo `dialout` e faça logout/login:
> `sudo usermod -a -G dialout $USER`

---

## 3. Bibliotecas do Arduino

Instale pela Arduino IDE (**Ferramentas → Gerenciar Bibliotecas**) ou pelo `arduino-cli`.

| Para quê | Biblioteca | Observação |
|---|---|---|
| Ler o cartão SD | `SD` | Já vem com a Arduino IDE |
| Tocar áudio do SD | `TMRpcm` | Toca arquivos `.wav` pelo pino PWM |
| Ler as tags NFC | `MFRC522` **ou** `Adafruit PN532` | Depende do módulo NFC usado (RC522 ou PN532) |
| Motor DC | nenhuma | Controle feito direto por PWM (`analogWrite`) |

Pelo `arduino-cli`:

```bash
arduino-cli lib install "SD" "TMRpcm"
arduino-cli lib install "MFRC522"          # se o módulo for RC522
# arduino-cli lib install "Adafruit PN532" # se o módulo for PN532
```

> Se alguém adicionar uma biblioteca nova ao código, **atualize esta tabela** para que os outros saibam instalar.

### Formato dos arquivos de áudio

Para a `TMRpcm`, os arquivos no cartão SD precisam estar em **WAV, 8 bits, mono, 16 kHz** (ou 8–32 kHz), com nomes curtos (formato 8.3, ex.: `musica1.wav`). Para converter, dá para usar o [Audacity](https://www.audacityteam.org/) ou o `ffmpeg`:

```bash
ffmpeg -i entrada.mp3 -ar 16000 -ac 1 -acodec pcm_u8 musica1.wav
```

> **Atenção à memória:** o Duemilanove tem só 2 KB de RAM. SD + áudio + NFC juntos ficam no limite, então evite `String` e use `F("texto")` em `Serial.print` para economizar memória.

---

## 4. FreeCAD (modelagem 3D)

Ferramenta usada para modelar as peças impressas em 3D. Baixe em [freecad.org/downloads](https://www.freecad.org/downloads.php).

| Sistema | Como instalar |
|---|---|
| Windows / macOS | Instalador no site oficial |
| Linux | AppImage do site oficial, ou `flatpak install flathub org.freecad.FreeCAD` |

Para exportar uma peça para impressão: selecione o corpo → **Arquivo → Exportar** → formato **STL Mesh (`.stl`)**.

---

## 5. Fatiador (para a impressora 3D)

O `.stl` precisa ser convertido em G-code no fatiador antes de imprimir. Use o que for compatível com a impressora do grupo:

- [UltiMaker Cura](https://ultimaker.com/software/ultimaker-cura/)
- [PrusaSlicer](https://www.prusa3d.com/page/prusaslicer_424/)

> Arquivos de G-code são específicos de cada impressora/configuração, então **não precisam ir para o repositório**. Versione apenas o `.FCStd` e o `.stl`.

---

## ✅ Checklist rápido

- [ ] Git instalado e configurado
- [ ] Repositório clonado, na branch `develop`
- [ ] Arduino IDE (ou `arduino-cli`) instalado, com a placa Duemilanove selecionada
- [ ] Bibliotecas `SD`, `TMRpcm` e a do módulo NFC instaladas
- [ ] FreeCAD instalado
- [ ] Fatiador instalado (se for imprimir)
