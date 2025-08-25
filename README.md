# verifica_qtd_chamadas
# Script desenvolvido em Python para monitorar a quantidade de canais. Ele obtém as informações a partir de um script em PHP e retorna o resultado em formato booleano, possibilitando o acompanhamento no Zabbix.
#!/usr/bin/python3.7

import os
import time                     # Bibliotecas para rodar nosso script
import datetime

status = 0

meu_log = "/etc/zabbix/scripts/logs_chamadas" # Caminho de onde iremos colocar nossos logs gerados
#meu_log = "/etc/zabbix/scripts/log_chamada.txt" # Caminho para testarmos o script

data_atual = datetime.datetime.today().strftime("%Y-%m-%d %H:%M:%S")

with open(meu_log, 'r') as arquivo:
    for linha in arquivo:
        partes = linha.split("=")
        ura = partes[0]
        valor = partes[1].strip()
        if valor == "": # Nessa etapa estamos verificando se op valor é igual a nulo usando o split e o strip para cortar as palavras geradas
            #print("#############################################") # Para visualizar o que está acontecendo batas descomentar essas 3 linhas 
            #print("VERIFIQUEM A:", ura )
            #print("#############################################")
            with open("/etc/zabbix/scripts/correcao.txt", 'a') as f:
                f.write(f"Verifiquem a ura: {ura} \n Arquivo verificado em: {data_atual}\n")
               
                status = 1
            continue
        
        qtd_chamadas = int(partes[1])
        if qtd_chamadas == 0: # Nessa etapa estamos verificando se o valor é igual a zero usando o split e o strip para cortar as palavras geradas
            #print("#############################################")
            #print("VERIFIQUEM A:", ura )
            #print("#############################################")
            with open("/etc/zabbix/scripts/correcao.txt", 'a') as g:
                g.write(f"Verifiquem a ura: {ura} \n Arquivo verificado em: {data_atual}\n")
                status = 1
print(status)

# Por fim ele retorna o valor de 0 para ok e 1 para PROBLEMA para conseguir mostrar no zabbix o alerta de alguma URA fora
