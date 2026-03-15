🛡️ Pentest Lab: Simulação de Ataques de Força Bruta com Medusa

Este projeto documenta a implementação de um ambiente controlado para simular ataques de força bruta em diferentes serviços (Web, FTP e SMB), utilizando o Kali Linux e a ferramenta Medusa contra a máquina vulnerável Metasploitable 2.

🛠️ Configuração do Ambiente
- Atacante: Kali Linux.
- Alvo: Metasploitable 2 (DVWA, FTP, SMB).
- Rede: VirtualBox com rede interna (host-only).
- Ferramenta Principal: Medusa.

![configuracao-do-ambiente](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/configuracao-do-ambiente.png)

![configuracao-do-ambiente](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/configuracao-do-ambiente.png)

🚀 Cenário 1: Ataque em Formulário Web (DVWA)
Nesta etapa, o objetivo foi automatizar tentativas de login em um formulário web típico, simulando o preenchimento em massa com múltiplas combinações.

1. Análise Técnica do Alvo
Antes de iniciar o ataque, foi necessário entender como os dados são enviados ao servidor através da barra de desenvolvedor do navegador (F12), especificamente na aba Network.

- URL do Alvo: http://172.30.0.101/dvwa/login.php.
- Método de Envio: POST.
- Parâmetros Identificados: username, password e o botão Login.

2. Identificação da String de Erro
Para que o Medusa saiba que o login falhou, identificamos que a aplicação retorna a frase "Login failed" quando as credenciais são incorretas
. Se a resposta não contiver essa frase, a ferramenta entende que o acesso foi bem-sucedido.

3. Execução com Medusa
O comando utilizado simulou as interações que um usuário teria no navegador, mas de forma automatizada.







--------------------------------------------------------------------------------
📂 Cenário 2: Força Bruta em FTP e SMB
(Aqui você deve documentar os outros dois ataques solicitados pelo desafio: o ataque ao serviço de transferência de arquivos e o password spraying em SMB).

--------------------------------------------------------------------------------
📊 Resultados e Validação
- Wordlists utilizadas: Listas simples com combinações de usuários e senhas comuns.
- Sucesso: Descrição de qual credencial foi capturada (ex: admin/password).
- Impacto: Se não mitigado, esse ataque pode levar ao comprometimento total do sistema, especialmente se o painelacessado for de administração ou controle de infraestrutura.

🛡️ Medidas de Mitigação
Para evitar que ataques reais ocorram, recomenda-se:
1. Políticas de Senhas Fortes: Evitar senhas curtas ou baseadas em dicionários.
2. Bloqueio de Conta (Account Lockout): Limitar o número de tentativas de login malsucedidas.
Autenticação de Dois Fatores (2FA): Adicionar uma camada extra de segurança além da senha.
3. Monitoramento: Analisar logs para identificar padrões de automação de tentativas de login em massa.

📁 Estrutura do Repositório
- /images: Capturas de tela do Medusa em execução e do acesso ao DVWA.
- /wordlists: Exemplos de listas de senhas utilizadas (não inclua senhas reais de uso pessoal!).
- README.md: Documentação principal do projeto.
