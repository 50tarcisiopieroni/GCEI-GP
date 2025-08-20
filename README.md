#  Sistema de Gestão para Centro de Educação Infantil

Este sistema está sendo desenvolvido com o objetivo de **auxiliar na gestão de um centro de educação infantil**, promovendo a organização de informações pedagógicas, administrativas e avaliativas.

O foco principal está em facilitar o trabalho da equipe escolar, oferecendo uma ferramenta digital prática e eficiente para o acompanhamento do desenvolvimento dos alunos e a rotina institucional.

## 🎯 Objetivo

O objetivo do sistema é oferecer uma ferramenta digital que facilite o dia a dia da equipe escolar, promovendo maior controle e organização das atividades pedagógicas e administrativas.

## 📦 Módulos Atuais

  - ✅ **Avaliação de alunos**: Permite registrar, acompanhar e consultar as avaliações dos alunos das turmas da educação infantil.

## 🛠️ Tecnologias utilizadas

  - ⚛️ **React** – Interface do usuário (frontend)
  - 🛠️ **Express** – Servidor e API (backend)
  - 🗄️ **Sequelize** – ORM para banco de dados relacional
  - 🐘 **PostgreSQL** – Banco de dados principal

## 🚀 Como executar

### ✅ Pré-requisitos

Antes de começar, é fundamental ter a versão correta do Node.js. Recomendamos o uso do **nvm** (Node Version Manager) para gerenciar as versões. Este projeto utiliza a versão **14.21**.

Se você não tiver o nvm, pode instalá-lo a partir do [repositório oficial](https://github.com/nvm-sh/nvm).

Caso esteja utilizando windows, é necessário instalar o nvm 1.1.2, pois a versão mais atual está tendo conflito com a versão do node 14.21
[Link de download do nmv-windows](https://github.com/coreybutler/nvm-windows/releases/tag/1.1.12)

Depois de instalar o nvm, execute os seguintes comandos no seu terminal para garantir que está usando a versão correta:

_Instale a versão 14.21 (caso ainda não a tenha)_
```bash
nvm install 14.21
```

_Use a versão 14_
```bash
nvm use 14
```

### ⚙️ Instalação

Com o ambiente configurado, clone o repositório:

```bash
git clone https://github.com/50tarcisiopieroni/GCEI-GP.git
```

> Apartir desse ponto, os comandos abaixos não serão referente aos códigos nesse repositório, mantido como exemplo

Antes de executar o projeto, copie os arquivos de modelo de variáveis de ambiente e renomeie para `.env`:

```bash
# Para o backend
cp ./backend/.env.example ./backend/.env

# Para o frontend
cp ./config/.env.example ./frontend/.env
```

Edite os arquivos `.env` conforme necessário, preenchendo os dados do banco de dados, porta, URLs, etc.

### Backend

```bash
cd backend/
npm install
npm run dev
```

Certifique-se de configurar as variáveis de ambiente (ex: `.env`) com os dados do PostgreSQL.

### Frontend

```bash
cd frontend/
npm install
npm start
```

## 🤝 Como Contribuir

Este é um projeto de desenvolvimento contínuo e aberto a contribuições. Se você tem interesse em ajudar, seja com código, documentação ou sugestões, por favor, leia nosso **[🚀Guia de Contribuição](CONTRIBUTING.md)** para começar.

> Agradecemos por qualquer feedback ou sugestão\! 💡
