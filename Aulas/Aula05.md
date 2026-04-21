# 🖥️ Guia Prático: Criando uma Máquina Virtual com antiX Linux

Se você quer testar um sistema operacional novo sem correr risco de estragar seu computador, esse guia é pra você. A ideia aqui é usar uma máquina virtual — basicamente um “computador dentro do computador” — pra rodar o **antiX Linux**, que é leve e rápido.

---

## 🧰 Etapa 1: Instalando o VirtualBox

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/0/0e/VirtualBox_logo_2016.png" width="160">
</div>

O **VirtualBox** é o programa que vai permitir criar essa máquina virtual.

### Passo a passo:

1. Abra seu navegador (Chrome, Edge, etc.).
2. Acesse: **https://www.virtualbox.org/**
3. Clique em **Download**.
4. Escolha a opção **Windows hosts** (caso use Windows).
5. Aguarde o download terminar.

### Instalando:

- Abra o arquivo baixado.
- Clique em **Next** até chegar na instalação.
- Vai aparecer um aviso sobre rede → pode clicar em **Yes** sem medo.
- Clique em **Install** e depois **Finish**.

✔️ Pronto! Seu VirtualBox já está funcionando.

---

## 📦 Etapa 2: Baixando o antiX Linux

<div align="center">
  <img src="https://antixlinux.com/wp-content/uploads/2020/07/antix-logo.png" width="200">
</div>

Agora vamos pegar o sistema que vamos instalar.

### Como fazer:

1. Acesse: **https://antixlinux.com/**
2. Clique em **Download** no menu.
3. Escolha um servidor (de preferência mais próximo).
4. Baixe a versão **antiX-full (.iso)**.

💡 Esse arquivo é como se fosse um “instalador completo” do sistema.

---

## 🏗️ Etapa 3: Criando a Máquina Virtual

Agora vamos montar nosso computador virtual.

1. Abra o VirtualBox.
2. Clique em **Novo**.

### Configurações:

- **Nome:** antiX
- **Tipo:** Linux
- **Versão:** Debian (64-bit)

### Memória RAM:
- Coloque **2048 MB (2GB)**

### Disco:
- Criar novo disco virtual
- Tipo: VDI
- Tamanho: entre **20GB e 40GB**

Clique em **Criar**.

---

## ⚙️ Etapa 4: Preparando o Sistema

Agora precisamos “colocar o sistema dentro da máquina”.

<div align="center">
  <img src="https://media.giphy.com/media/kH1DBkPNyZPOk0BxrM/giphy.gif" width="400">
</div>

### Passos:

1. Selecione a máquina criada.
2. Clique em **Configurações**.
3. Vá em **Armazenamento**.
4. Clique em **Vazio**.
5. Clique no ícone de disco → **Escolher arquivo**.
6. Selecione o arquivo `.iso` do antiX.

Clique em **OK**.

---

## 🚀 Etapa 5: Iniciando a Máquina

Agora vem a parte legal 😄

1. Clique em **Iniciar** (seta verde).
2. A máquina virtual vai ligar.
3. O sistema antiX será carregado automaticamente.

<div align="center">
  <img src="https://media.giphy.com/media/QTfX9Ejfra3ZmNxh6B/giphy.gif" width="400">
</div>

### O que fazer agora?

- Escolher idioma
- Configurar teclado
- Criar usuário

✔️ Depois disso, o sistema já estará pronto para uso!

---

## 🎯 Conclusão

Usar máquina virtual é uma forma segura e prática de testar sistemas operacionais.

Você pode:
- Testar Linux sem apagar nada
- Aprender mais sobre sistemas
- Treinar sem medo de errar

---

<div align="center">
  <img src="https://media.giphy.com/media/3o7TKtnuHOHHUjR38Y/giphy.gif" width="300">
  <p><strong>Agora é só explorar o sistema e aprender na prática! 🚀</strong></p>
</div>
