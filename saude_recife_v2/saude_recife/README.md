# 🏥 Saúde Recife App — v2.0

Aplicativo mobile desenvolvido em **React Native (Expo)** que localiza unidades de saúde próximas ao usuário na cidade do Recife, integrando dados abertos da Prefeitura com geolocalização e um backend próprio para check-ins e avaliações.

---

## 🎯 Funcionalidades

| Funcionalidade | Status |
|---|---|
| Geolocalização do usuário (GPS) | ✅ |
| Busca de clínicas na API do Dados Recife | ✅ |
| Filtro por proximidade (raio de 10 km) | ✅ |
| Exibição de horários e informações | ✅ |
| Check-in com avaliação (1–5 estrelas) | ✅ |
| Feedback textual por visita | ✅ |
| Histórico de check-ins (banco de dados) | ✅ |
| Remoção de check-ins | ✅ |

---

## 🗂️ Estrutura do Projeto

```
.
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── db.js               # Conexão com MongoDB
│   │   ├── controllers/
│   │   │   ├── CheckinController.js  # Lógica de check-ins (GET, POST, DELETE)
│   │   │   └── UsuarioController.js  # Lógica de usuários
│   │   ├── models/
│   │   │   ├── Checkin.js           # Schema do check-in (com avaliação e feedback)
│   │   │   └── Usuario.js           # Schema do usuário
│   │   └── server.js               # Servidor Express + rotas
│   └── package.json
│
└── mobile/
    ├── App.js                       # Navegação entre telas
    ├── src/
    │   ├── screens/
    │   │   ├── HomeScreen.js        # Lista de clínicas + modal de check-in
    │   │   ├── HistoryScreen.js     # Histórico de check-ins
    │   │   ├── LoginScreen.js       # Tela de login
    │   │   └── CadastroScreen.js   # Tela de cadastro
    │   └── services/
    │       ├── dadosRecifeApi.js    # Integração com API pública do Recife
    │       └── backendApi.js        # Integração com nosso backend
    └── package.json
```

---

## 🌐 API Externa — Dados Recife

**Endpoint utilizado:**
```
GET https://dados.recife.pe.gov.br/api/3/action/datastore_search
    ?resource_id=c727e8f8-40e9-415e-b14d-2c46406abb60
    &limit=100
```

**Por que esse endpoint?**
- Contém todas as **Unidades de Saúde do Município do Recife** (UBS, NASF, Policlínicas)
- Retorna campos de `latitude` e `longitude`, essenciais para o filtro por proximidade
- Inclui `telefone`, `distrito`, `área`, `horario` e `ponto_de_apoio` para exibição ao usuário
- É mantido pela Prefeitura do Recife e atualizado regularmente
- Acesso público e gratuito, sem necessidade de autenticação

---

## 🔧 Rotas do Backend (Node.js + Express + MongoDB)

| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/` | Health check — verifica se o servidor está online |
| `GET` | `/checkins` | **Lista todos os check-ins** (histórico) |
| `GET` | `/checkins/:id` | Busca um check-in específico por ID |
| `POST` | `/checkins` | **Cria um novo check-in** com localização e feedback |
| `DELETE` | `/checkins/:id` | Remove um check-in pelo ID |
| `POST` | `/usuarios` | Cadastra um novo usuário |
| `POST` | `/usuarios/login` | Autentica um usuário |
| `GET` | `/usuarios` | Lista usuários (uso administrativo) |

### Exemplo de POST `/checkins`

**Body (JSON):**
```json
{
  "localNome": "UBS Dois Irmãos",
  "latitudeUsuario": -8.0476,
  "longitudeUsuario": -34.8770,
  "distrito": "DS I - Noroeste",
  "area": "Dois Irmãos",
  "telefone": "(81) 3355-0000",
  "avaliacao": 4,
  "feedback": "Atendimento rápido e equipe atenciosa!"
}
```

**Resposta (201 Created):**
```json
{
  "_id": "664f3a2b...",
  "localNome": "UBS Dois Irmãos",
  "latitudeUsuario": -8.0476,
  "longitudeUsuario": -34.8770,
  "avaliacao": 4,
  "feedback": "Atendimento rápido e equipe atenciosa!",
  "dataRegistro": "2026-06-18T14:30:00.000Z"
}
```

---



## 🛠️ Tecnologias Utilizadas

**Mobile:**
- React Native + Expo
- expo-location (geolocalização)
- @react-navigation/native (navegação)
- axios (requisições HTTP)

**Backend:**
- Node.js + Express
- MongoDB + Mongoose
- CORS

**API Externa:**
- Portal de Dados Abertos do Recife (CKAN)

---

