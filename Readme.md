<div align="center">

# Erik Silva
### Full Stack Developer - React · Next.js · Node.js · Automações (n8n)

Construo sistemas de ponta a ponta - do modelo de dados à interface - priorizando
segurança, manutenibilidade e produtos que rodam com dinheiro real envolvido.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-erik--borgessilva20-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/erik-borgessilva20)
[![Email](https://img.shields.io/badge/Email-erik.silvadesenvolvedor%40gmail.com-D14836?style=flat&logo=gmail&logoColor=white)](mailto:erik.silvadesenvolvedor@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-ErikBdaSilva20-181717?style=flat&logo=github&logoColor=white)](https://github.com/ErikBdaSilva20)

🇧🇷 [Português](#-sobre) · 🇺🇸 [English](#-about)

</div>

---

## 🇧🇷 Sobre

Há dois anos construindo sistemas full stack - do banco de dados à interface,
com foco em código que não vira dívida técnica depois de três meses. Trabalho como PJ
para um cliente fixo, além de projetos próprios e em parceria.

O que me interessa não é só "fazer funcionar" - é entender onde o sistema quebra
sob uso real e corrigir antes que isso vire prejuízo pra alguém. O case abaixo
(Marketing Lab) mostra isso na prática: uma auditoria que eu mesmo conduzi contra
banco de produção, achando e corrigindo falhas que os testes automatizados não pegavam.

### Stack

![React](https://img.shields.io/badge/React-20232A?style=flat&logo=react&logoColor=61DAFB)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=nodedotjs&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat&logo=supabase&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat&logo=mongodb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat&logo=n8n&logoColor=white)

**Frontend:** React, Next.js, TypeScript, Tailwind, Styled Components, shadcn/ui, TanStack <br/>
**Backend:** Node.js, Fastify, Prisma, PostgreSQL, MongoDB, Convex, Supabase (RLS/Auth/Realtime/Edge Functions<br/>
**Automação:** n8n, bots para Telegram/WhatsApp, integrações via API<br/>
**Ferramentas:** Git, Docker

---

## 💼 Projeto em destaque - Marketing Lab

**Plataforma de e-learning full stack** com trilhas de curso, gamificação, comunidade
em tempo real e um modelo de monetização por professor - sistema que roda uma
agência real que vende cursos e paga seus criadores por visualização qualificada.

Alunos se matriculam, assistem aula, fazem exercício e quiz com correção automática,
ganham pontos/badges/certificado e participam de uma comunidade com chat em tempo real.
Professores têm painel próprio com métricas e ganhos financeiros, isolados dos dados de
outros professores. Dois super admins administram a plataforma inteira, incluindo a
apuração financeira mensal entre criadores.

**Decisão de arquitetura central:** toda autorização vive no banco (Postgres + Row Level
Security), nunca só em guard de rota no client - então mesmo uma chamada direta à API,
contornando a UI, respeita as mesmas regras que o app usa.

`React 18 · TypeScript · Vite · Supabase (Postgres/RLS/Auth/Realtime/Edge Functions) · TanStack Query · Vitest · Playwright · pgTAP`

<details>
<summary><b>🔍 Três falhas reais que encontrei e corrigi em produção (clique para expandir)</b></summary>

<br>

Rodei uma auditoria manual logado em cinco contas diferentes contra o Supabase de
produção - não só a suíte automatizada. Isso expôs problemas que os testes não cobriam,
porque a superfície vulnerável (views do banco) nunca tinha sido testada com a mesma
rigidez que as tabelas.

**1. Views do banco sem trava de permissão - dado pessoal exposto a visitante anônimo**
Quatro views que juntavam dados de várias tabelas (progresso, ranking, conquistas)
foram criadas sem `security_invoker` e com escrita liberada por padrão. Qualquer
visitante sem login conseguia ler dados pessoais de todos os usuários, e uma view
permitia apagar alternativas de quiz sem autenticação - comprovei apagando (e
restaurando) uma alternativa real com cliente anônimo. Corrigido ligando a trava de
permissão e revogando escrita indevida; depois criei testes cobrindo view por view,
já que a suíte de RLS existente só testava tabelas.

**2. Aluno conseguia gravar a própria nota no exercício**
A correção de múltipla escolha rodava no navegador do aluno, que calculava a nota e
gravava direto no banco. Provei o problema mandando um aluno gravar nota 100 numa prova
em branco - funcionou, e essa nota alimentava desbloqueio de curso e badges de maestria.
Corrigido movendo a correção para uma função `SECURITY DEFINER` no banco, que roda uma
vez por aluno/prova sem confiar em nada vindo do client.

**3. Apuração de pagamento a professores nunca "fechava" o mês**
Um gatilho de proteção contra adulteração pelo navegador também bloqueava a própria
rotina de apuração de marcar visualizações como pagas - resultado: cada execução
reprocessava tudo desde o início. Testei o risco real (apaguei um pagamento já feito e
rodei de novo): **pagou em duplicidade**. Corrigido fazendo o gatilho distinguir escrita
do client de escrita da própria rotina interna, usando sinalizador de sessão já usado
em outros pontos sensíveis do sistema. Revalidei contra produção repetindo o cenário -
não pagou de novo - e adicionei testes permanentes para os dois lados.

</details>


### Nonno English
Plataforma de gestão e aprendizado para escola de inglês, dividida em três pilares:
landing page pública, portal do aluno (materiais, atividades, progresso) e painel do
professor com ERP educacional completo — gestão de alunos, agendamento de aulas e
módulo financeiro (mensalidades, fluxo de caixa, gráficos de performance).

`React (Vite) · Supabase (Postgres + Auth + Realtime) · Styled Components · Recharts`

<details>
<summary><b>💎 Destaques técnicos</b></summary>

<br>

- **Performance em dashboards complexos:** uso de `useMemo`/`useCallback` para evitar
  re-renderizações desnecessárias nas telas financeiras e de gestão do professor.
- **Arquitetura modular:** componentes reutilizáveis e layouts separados por perfil
  (`AdminLayout`, `PublicLayout`), facilitando manutenção conforme a plataforma cresce.
- **Rotas protegidas:** autenticação e controle de acesso via Supabase, isolando dados
  sensíveis (financeiro e de alunos) por perfil de usuário.
- **Design system próprio:** paleta Forest Green & Gold, tipografia Inter, glassmorphism
  e micro-animações — pensado para transmitir a sensação de produto premium, não só
  "sistema administrativo".

</details>

---

## 🇺🇸 About

Two years building full stack systems - from database to interface, focused on code
that doesn't become technical debt three months later. I work as an independent
contractor for a fixed client, alongside my own and partnered projects.

What drives me isn't just "making it work" - it's understanding where a system breaks
under real usage and fixing it before it costs someone money. The case study below
(Marketing Lab) shows this in practice: a self-run audit against a production database
that found and fixed issues automated tests never caught.

### Stack
**Frontend:** React, Next.js, TypeScript, Tailwind, Styled Components, shadcn/ui, TanStack
**Backend:** Node.js, Fastify, Prisma, PostgreSQL, MongoDB, Convex, Supabase (RLS/Auth/Realtime/Edge Functions)
**Automation:** n8n, Telegram/WhatsApp bots, API integrations
**Tools:** Git, Docker

### Featured project — Marketing Lab

Full stack e-learning platform with course tracks, gamification, real-time community,
and a per-teacher monetization model - a system that runs a real agency that sells
courses and pays creators by qualified view. **Core architecture decision:** all
authorization lives in the database (Postgres Row Level Security), never only in
client-side route guards - so even a direct API call that bypasses the UI still
respects the same rules the app uses.

`React 18 · TypeScript · Vite · Supabase (Postgres/RLS/Auth/Realtime/Edge Functions) · TanStack Query · Vitest · Playwright · pgTAP`

<details>
<summary><b>🔍 Three real production issues I found and fixed (click to expand)</b></summary>

<br>

I ran a manual audit logged into five different accounts against the production
Supabase instance - not just the automated suite. This surfaced issues the tests
didn't cover, because the vulnerable surface (database views) had never been tested
as rigorously as the tables.

**1. Unprotected database views exposed personal data to anonymous visitors**
Four views were created without `security_invoker` and with write access open by
default - any logged-out visitor could read every user's personal data, and one view
allowed deleting quiz answers without authentication. Verified by deleting (and
restoring) a real answer via an anonymous client. Fixed by enabling the permission
lock and revoking improper write access; added per-view tests since the existing RLS
suite only covered tables.

**2. Students could self-grade their own multiple-choice exercises**
Grading ran client-side - the browser computed the score and wrote it straight to the
database. Proved it by having a student write a 100% score on a blank exam; that score
fed course unlock logic and mastery badges. Fixed by moving grading into a
`SECURITY DEFINER` database function that runs once per student/exam without trusting
anything from the client.

**3. Teacher payout reconciliation never "closed" the month**
An anti-tampering trigger meant to block browser writes was also blocking the payout
routine itself from marking views as paid - every run reprocessed everything from day
one. Tested the real risk (deleted an already-paid record and reran): **it paid twice**.
Fixed by making the trigger distinguish client writes from the routine's own internal
writes, using a session flag already used elsewhere in the system. Reverified against
production and added permanent regression tests for both paths.

</details>

### Nonno English
Management and learning platform for an English school, split into three pillars: a
public landing page, a student portal (materials, assignments, progress tracking), and
a teacher dashboard with a full educational ERP - student management, class scheduling,
and a financial module (tuition, cash flow, performance charts).

`React (Vite) · Supabase (Postgres + Auth + Realtime) · Styled Components · Recharts`

<details>
<summary><b>💎 Technical highlights</b></summary>

<br>

- **Performance in complex dashboards:** `useMemo`/`useCallback` used to avoid
  unnecessary re-renders on the teacher's financial and management screens.
- **Modular architecture:** reusable components and layouts split by user role
  (`AdminLayout`, `PublicLayout`), easing maintenance as the platform grows.
- **Protected routes:** authentication and access control via Supabase, isolating
  sensitive data (financial and student records) by user role.
- **Custom design system:** Forest Green & Gold palette, Inter typeface,
  glassmorphism and micro-animations - built to feel like a premium product, not
  just an admin system.

</details>

<p align="center"><i>Sistemas que resolvem problema real, não só código que compila.</i></p>
