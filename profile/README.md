<div align="center">

<img src="https://github.com/Valence-Team.png" width="110" alt="Logo do Valence">

# Valence

**Rede pública e colaborativa de vigilância fitossanitária para pequenos produtores**

![FIAP](https://img.shields.io/badge/FIAP-Global_Solution_2026%2F2-0B1F3A?style=for-the-badge)
![CitizenTech](https://img.shields.io/badge/Tema-CitizenTech-22C55E?style=for-the-badge)
![Culturas](https://img.shields.io/badge/Milho_·_Tomate_·_Batata-2F80ED?style=for-the-badge)

</div>

---

## O problema

Comunidades rurais isoladas descobrem pragas tarde, sem técnico por perto e sem um canal rápido entre vizinhos e poder público. A FAO estima que cerca de **40% da produção agrícola mundial** se perde para pragas. Só a cigarrinha-do-milho causa prejuízo estimado de **US$ 6,5 bilhões por ano** no Brasil (Embrapa, 2026). A perda chega ao produtor e também ao consumidor, no preço e na oferta.

## Como o Valence funciona

1. **Registro.** O produtor fotografa a folha danificada e descreve o problema pelo app.
2. **Triagem.** A IA analisa a imagem, consulta fichas técnicas e sugere hipóteses de praga, severidade e cuidados imediatos. É triagem, não diagnóstico.
3. **Alerta.** Propriedades vizinhas com a mesma cultura, num raio de 10 km, recebem um alerta de suspeita.
4. **Laudo.** Um agrônomo confirma ou descarta o caso. Só ele transforma a suspeita em foco confirmado.
5. **Dados abertos.** Os focos viram dados anonimizados no Observatório Fitossanitário, abertos ao poder público e a qualquer cidadão.

Além disso, o **mural da região** permite que produtores conversem sobre a situação local, com denúncias analisadas por IA e moderação humana.

> Apps de diagnóstico olham para uma lavoura de cada vez. O Valence é **coletivo**: tem validação humana e gera dado para política pública.

## Arquitetura

```mermaid
flowchart LR
    A[App Mobile<br/>React Native + Expo] -->|REST + JWT| B[API Java<br/>Spring Boot]
    B -->|HTTP| C[Serviço de IA<br/>Python + FastAPI]
    B -->|Spring AI| D[LLM<br/>Groq / Gemini]
    B -->|JPA| E[(Azure SQL)]
    B -->|eventos| F{{RabbitMQ}}
    F --> G[Observatório<br/>.NET Minimal API]
    G --> H[(Oracle)]
    G -->|HTTP| I[API do IBGE]
```

**Princípios**

- O app conversa só com a API Java.
- Java e .NET se integram apenas por eventos. Se o Observatório estiver fora do ar, as mensagens esperam na fila.
- A API continua funcionando sem a IA: o caso vai direto para a fila do agrônomo.
- Nenhum dado pessoal sai da API Java. Eventos e prompts levam só dados anonimizados.
- A IA nunca decide sozinha: todo alerta automático é suspeita, e só a moderação humana remove um post.

## Repositórios

| Repositório | O que é | Disciplina |
| --- | --- | --- |
| [valence-java](https://github.com/Valence-Team/valence-java) | API núcleo: ocorrências, triagem com Spring AI, alertas, laudos e mural | Java Advanced · DevOps |
| [valence-mobile](https://github.com/Valence-Team/valence-mobile) | App para produtores, agrônomos e gestores | Mobile Application Development |
| [valence-iot](https://github.com/Valence-Team/valence-iot) | Serviço de IA: visão computacional e RAG sobre fichas técnicas | Disruptive Architectures |
| [valence-dotnet](https://github.com/Valence-Team/valence-dotnet) | Observatório Fitossanitário com dados abertos e boletins | .NET |
| [valence-database](https://github.com/Valence-Team/valence-database) | Regras em Oracle PL/SQL e transição para MongoDB | Banco de Dados |
| [valence-qa](https://github.com/Valence-Team/valence-qa) | Backlog Scrum e casos de teste funcionais | Compliance, QA & Tests |

O CI/CD da API (pipeline YAML, scripts Azure CLI e Dockerfile) fica no `valence-java`, espelho público do repositório no Azure Repos.

## Tecnologias

![Java](https://img.shields.io/badge/Java_21-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![React Native](https://img.shields.io/badge/React_Native-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Expo](https://img.shields.io/badge/Expo-000020?style=flat-square&logo=expo&logoColor=white)
![.NET](https://img.shields.io/badge/.NET_8-512BD4?style=flat-square&logo=dotnet&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle-F80000?style=flat-square&logo=oracle&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat-square)
![Azure DevOps](https://img.shields.io/badge/Azure_DevOps-0078D7?style=flat-square)

## Impacto

| ODS | Contribuição |
| --- | --- |
| 2 · Fome zero e agricultura sustentável | Reduz perdas de safra com detecção precoce |
| 9 · Indústria, inovação e infraestrutura | Leva IA e dados ao campo com baixo custo |
| 10 · Redução das desigualdades | Dá ao pequeno produtor isolado acesso a um técnico |
| 13 · Ação contra a mudança do clima | Monitora pragas que se espalham com o clima |

## Equipe

| Integrante | RM | Frentes |
| --- | --- | --- |
| Henrique Martins | 563620 | API Java, App Mobile, Serviço de IA e DevOps |
| Guilherme Macedo | 562396 | Observatório .NET e Banco de Dados |
| Pedro Henrique | 563405 | Qualidade, testes e compliance |

<div align="center">
<sub>FIAP · Análise e Desenvolvimento de Sistemas · Global Solution 2026/2</sub>
</div>
