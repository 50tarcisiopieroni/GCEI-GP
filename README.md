

<p align="center">
  <img src="frontend/public/logo.svg" alt="Logo Instituto Espírita Eurípedes" width="200"\>
</p\>

#  Sistema de Gestão para Centro de Educação Infantil

Este sistema está sendo desenvolvido pelo **Instituto Espírita Eurípedes** com o objetivo de **auxiliar na gestão de um centro de educação infantil**, promovendo a organização de informações pedagógicas, administrativas e avaliativas.

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

Antes de começar, é fundamental ter a versão correta do Node.js. Recomendamos o uso do **nvm** (Node Version Manager) para gerenciar as versões. Este projeto utiliza a versão **14.2**.

Se você não tiver o nvm, pode instalá-lo a partir do [repositório oficial](https://github.com/nvm-sh/nvm).

Depois de instalar o nvm, execute os seguintes comandos no seu terminal para garantir que está usando a versão correta:

```bash
# Instala a versão 14.2 (caso ainda não a tenha)
nvm install 14.2

# Ativa a versão 14.2 para a sessão atual do terminal
nvm use 14.2
```

### ⚙️ Instalação

Com o ambiente configurado, clone o repositório:

```bash
git clone https://github.com/50tarcisiopieroni/Projeto-IEE.git
```

Antes de executar o projeto, copie os arquivos de modelo de variáveis de ambiente e renomeie para `.env`:

```bash
# Para o backend
cp ./config/back-model.env ./backend/.env

# Para o frontend
cp ./config/front-model.env ./frontend/.env
```

Edite os arquivos `.env` conforme necessário, preenchendo os dados do banco de dados, porta, URLs, etc.

### Backend

```bash
cd backend/
npm install
npm start
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
