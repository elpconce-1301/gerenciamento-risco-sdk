
<h1 style="color:#6A0DAD; font-family:Segoe UI, Arial; font-size:30px;">
🛡️ Riscos Identificados — Projeto Violeta SDK
</h1>

<p style="font-size:15px;">
Este documento apresenta os principais riscos identificados no projeto real de integração do 
<strong>SDK Violeta</strong> com o catálogo corporativo de APIs, considerando aspectos técnicos, 
de segurança, arquitetura, operação e uso de Inteligência Artificial Generativa.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🔐 1. Riscos de Segurança e Autenticação
</h2>

<h3 style="color:#4B0082;">🔸 Risco 1 — Exposição indevida do PAT</h3>
<p>
O desenvolvedor utiliza um <strong>Personal Access Token</strong> para instalar o SDK privado. 
PATs podem ser expostos em logs, histórico da IDE ou commits acidentais, permitindo que terceiros 
instalem o SDK sem autorização.
</p>

<h3 style="color:#4B0082;">🔸 Risco 2 — Autenticação sistêmica frágil (usuário/senha Base64)</h3>
<p>
A biblioteca envia credenciais sistêmicas encodadas em Base64 para o serviço <code>ms-agents-registry</code>. 
Base64 não é criptografia, permitindo que credenciais sejam facilmente recuperadas em caso de interceptação.
</p>

<h3 style="color:#4B0082;">🔸 Risco 3 — Acesso irrestrito ao catálogo de APIs</h3>
<p>
Qualquer pessoa que instale o SDK consegue consultar o catálogo interno de APIs, pois não há autenticação 
individualizada ou controle de acesso granular.
</p>

<h3 style="color:#4B0082;">🔸 Risco 4 — Dependência crítica de usuário sistêmico único</h3>
<p>
Se o usuário sistêmico for bloqueado ou tiver senha rotacionada, todo o SDK deixa de funcionar, causando 
indisponibilidade total.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
⚙️ 2. Riscos Técnicos e de Arquitetura
</h2>

<h3 style="color:#4B0082;">🔸 Risco 5 — Acoplamento forte entre SDK e serviços internos</h3>
<p>
O SDK depende diretamente de múltiplos serviços internos (ms-agents-registry, ms-auth-api, OIM). 
Falhas em qualquer camada impactam a experiência do desenvolvedor.
</p>

<h3 style="color:#4B0082;">🔸 Risco 6 — Latência elevada nas consultas via LLM</h3>
<p>
O fluxo de autenticação e consulta envolve diversas chamadas encadeadas, aumentando a latência e 
prejudicando a experiência na IDE.
</p>

<h3 style="color:#4B0082;">🔸 Risco 7 — Sincronização inconsistente entre catálogo oficial e base interna</h3>
<p>
Se o backend que sincroniza as APIs falhar ou atrasar, o SDK pode retornar informações desatualizadas 
ou incorretas.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🧠 3. Riscos Relacionados ao Uso de IA Generativa
</h2>

<h3 style="color:#4B0082;">🔸 Risco 8 — Alucinação do modelo ao sugerir APIs</h3>
<p>
LLMs podem sugerir APIs inexistentes, interpretar incorretamente a intenção do desenvolvedor ou gerar 
exemplos inválidos.
</p>

<h3 style="color:#4B0082;">🔸 Risco 9 — Exposição de informações internas via prompts</h3>
<p>
Perguntas abertas podem levar o modelo a retornar detalhes internos do catálogo, expondo endpoints 
sensíveis ou APIs não destinadas ao público geral.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
📦 4. Riscos Operacionais
</h2>

<h3 style="color:#4B0082;">🔸 Risco 10 — Falta de governança sobre quem usa o SDK</h3>
<p>
Sem autenticação individual, não há métricas de uso, auditoria ou rastreabilidade de acessos, dificultando 
a identificação de incidentes.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
📌 Resumo dos Riscos Principais
</h2>

<ul>
  <li>Exposição indevida de PATs.</li>
  <li>Autenticação sistêmica frágil (Base64).</li>
  <li>Acesso irrestrito ao catálogo de APIs.</li>
  <li>Dependência
