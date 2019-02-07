Questão 1 - Inclua o registro MX (mail xchanger) no respectivo arquivo (db.) do Bind. 

A alteração foi feita utilizando a máquina AUTO no arquivo db.org.pixel, onde foram incluídas as seguintes linhas:

@ IN MX 5 mail.pixel.org
mail IN A 192.168.1.2
imap IN A 192.168.1.2

Tendo feito isso, as máquinas AUTO, TLD, ROOT e LOCAL foram inicializadas com o comando:

$ /etc/init.d/bind9 start

Após isso foi verificada a resolução dos novos registros nas máquinas cliente (C1,C2,C3) utilizando o ping.

$ ping mail.pixel.org

Configure o servidor SMTP. Para isso, insira o comando no terminal
AUTO: $ dpkg-reconfigure exim4-config

Esse comando abrirá um assistente de configuração do servidor SMTP exim4. As configurações são:

General type of mail configuration:  internet site; mail is sent and received directly using SMTP.
System mail name: pixel.org
IP-addresses to listen on for incomming SMTP connections: // leave blank
Other destinations for which mail is accepted: pixel.org
Domains to relay mail for: // leave blank
Machines to relay mail for: // leave blank
Keep number of DNS-queries minimal (Dial-on-Demand) ?: No
Delivery method for local mail: mbox format in /var/mail/
Split configuration into small files ? : No

*para extrair as configurações:
AUTO: $ cd /etc
AUTO: $ mkdir exim4
AUTO: $ cp /etc/exim4/update-exim4.conf.conf /hostlab/AUTO/etc/exim4/

Para iniciar os servidores:
AUTO: $ /etc/init.d/bind9 restart
AUTO: $ /etc/init.d/exim4 restart
AUTO: $ /etc/init.d/openbsd-inetd restart

QUESTÃO 2

Numa das máquinas cliente, teste o envio e recebimento de e-mail via telnet.

Na máquina AUTO se criam dois usuários que irão conversar entre si (enviar e receber e-mail)
AUTO: $ adduser allan
password: 1234
AUTO: $ adduser joao
password 1234

Na máquina cliente:

C: $ telnet mail.pixel.org 25
C: $ HELO mail.pixel.org
C: $ MAIL FROM: <allan@pixel.org>
C: $ RCPT TO: <joao@pixel.org>
DATA
- put your message here -
.
quit

Recebimento com POP3:

No servidor, é dado o seguinte comando:

S: $telnet imap.pixel.org 110
S: $user joao
S: $pass 1234
S: $list //abre a caixa de entrada do usuário joao
S: $retr 1 //lê o e-mail de número 1
S: $quit
