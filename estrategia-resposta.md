<h1 style="color:#6A0DAD; font-family:Segoe UI, Arial; font-size:30px;">
🛡️ Estratégia de Resposta — Autenticação Sistêmica Frágil
</h1>

<p style="font-size:15px;">
Este documento apresenta a estratégia de resposta aprofundada para um dos riscos críticos identificados no projeto Violeta SDK: 
o uso de credenciais sistêmicas encodadas em Base64 para autenticação entre o SDK e o serviço interno 
<code>ms-agents-registry</code>.  
A mitigação deste risco é essencial para garantir segurança, governança e continuidade operacional.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🔍 1. Descrição do Risco
</h2>

<p>
Atualmente, o SDK Violeta envia um usuário sistêmico (<code>SVC_AGENTES_XXXX</code>) e senha encodados em Base64 para o 
serviço <code>ms-agents-registry</code>. Como Base64 não é criptografia, qualquer interceptação da requisição permite 
recuperar as credenciais.  
Além disso, credenciais sistêmicas costumam ter permissões amplas, ampliando o impacto de um eventual vazamento.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🔥 2. Impacto no Projeto
</h2>

<ul>
  <li>Acesso indevido ao catálogo interno de APIs.</li>
  <li>Possibilidade de escalonamento de privilégios.</li>
  <li>Violação de políticas internas de segurança e auditoria.</li>
  <li>Interrupção total do SDK caso o usuário sistêmico seja bloqueado ou tenha senha rotacionada.</li>
  <li>Risco reputacional para a área de arquitetura e IA.</li>
</ul>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🎯 3. Estratégia de Resposta (Mitigação)
</h2>

<p>
O objetivo da estratégia é eliminar o uso de credenciais sistêmicas fixas e substituí-las por um modelo seguro, 
individualizado e auditável, reduzindo riscos de segurança e aumentando a confiabilidade do SDK.
</p>

---

<h3 style="color:#4B0082;">🔸 Ação 1 — Autenticação baseada em tokens individuais (OAuth2 / Client Credentials)</h3>
<p>
Substituir o usuário sistêmico por um fluxo de autenticação baseado em <strong>Client ID / Client Secret</strong> 
por desenvolvedor, ou por <strong>Managed Identity</strong> quando aplicável.
</p>
<p><strong>Benefícios:</strong></p>
<ul>
  <li>Identidade individual para cada desenvolvedor.</li>
  <li>Auditoria completa de acessos.</li>
  <li>Revogação granular sem impacto global.</li>
  <li>Eliminação de credenciais fixas trafegando na rede.</li>
</ul>

---

<h3 style="color:#4B0082;">🔸 Ação 2 — Criar um serviço intermediário de autenticação</h3>
<p>
Implementar um backend que receba a identidade do desenvolvedor, valide permissões e emita um 
<strong>token temporário (JWT)</strong> para o SDK.
</p>
<p><strong>Benefícios:</strong></p>
<ul>
  <li>Centralização da segurança.</li>
  <li>Tokens temporários reduzem impacto de vazamento.</li>
  <li>Possibilidade de rate limiting e monitoramento.</li>
</ul>

---

<h3 style="color:#4B0082;">🔸 Ação 3 — Criptografar todas as credenciais em trânsito</h3>
<p>
Garantir que o SDK utilize <strong>TLS 1.2+</strong> e que nenhuma credencial trafegue em Base64.  
Tokens devem ser assinados e possuir expiração automática.
</p>

---

<h3 style="color:#4B0082;">🔸 Ação 4 — Rotação automática de segredos</h3>
<p>
Caso ainda existam segredos sistêmicos temporários, eles devem ser armazenados em <strong>Key Vault</strong> e rotacionados 
automaticamente, sem trafegar em requisições HTTP.
</p>

---

<h3 style="color:#4B0082;">🔸 Ação 5 — Auditoria e monitoramento de acessos</h3>
<p>
Registrar quem acessou o catálogo, quando, quais APIs foram consultadas e volume de chamadas.  
Permite identificar uso indevido e gerar métricas de evolução.
</p>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
📈 4. Plano de Ação
</h2>

<table>
  <tr>
    <th>Etapa</th>
    <th>Ação</th>
    <th>Responsável</th>
    <th>Prazo</th>
    <th>Status</th>
  </tr>
  <tr>
    <td>1</td>
    <td>Definir modelo de autenticação individual</td>
    <td>Arquitetura + Segurança</td>
    <td>2 semanas</td>
    <td>Planejado</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Criar serviço intermediário de autenticação</td>
    <td>Backend</td>
    <td>4 semanas</td>
    <td>Planejado</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Atualizar SDK para usar tokens temporários</td>
    <td>Time do SDK</td>
    <td>3 semanas</td>
    <td>Planejado</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Implementar auditoria e logs</td>
    <td>Observabilidade</td>
    <td>2 semanas</td>
    <td>Planejado</td>
  </tr>
  <tr>
    <td>5</td>
    <td>Descontinuar usuário sistêmico</td>
    <td>Segurança</td>
    <td>Após implantação</td>
    <td>Planejado</td>
  </tr>
</table>

---

<h2 style="color:#4B0082; font-family:Segoe UI, Arial;">
🧩 5. Justificativa da Estratégia
</h2>

<p>
A mitigação deste risco é particularmente importante porque o uso de credenciais sistêmicas fixas representa um 
risco crítico para segurança, governança e continuidade operacional.  
Ao migrar para autenticação individualizada e tokens temporários, o projeto passa a ter:
</p>

<ul>
  <li>Maior controle de acesso.</li>
  <li>Rastreabilidade completa.</li>
  <li>Conformidade com padrões corporativos.</li>
  <li>Redução de impacto em caso de vazamento.</li>
  <li>Maior robustez operacional.</li>
</ul>

---

<p style="font-size:14px; color:#555;">
Este documento faz parte da atividade prática da pós-graduação em Inteligência Artificial.
</p>
