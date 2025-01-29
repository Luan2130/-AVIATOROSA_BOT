import time
from datetime import datetime, timedelta
from telegram import Bot, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.error import TelegramError

# Defina seu token e a lista de IDs dos grupos
TOKEN = "7521439465:AAHbKV_aGpubDyJm5-qjJvXnZJ5xKjURJc4"
CHAT_IDS = ["-1002371883642", "-1001598650040", "-1002156265337", "-1001677800886", "-1001551119198"]

# Cria o bot
bot = Bot(token=TOKEN)

# Função para enviar mensagens com tratamento de erros
def enviar_mensagem(chat_id, mensagem, botao_texto=None, url_botao=None):
    try:
        if botao_texto and url_botao:
            botoes = InlineKeyboardMarkup([[InlineKeyboardButton(botao_texto, url=url_botao)]])
            bot.send_message(chat_id=chat_id, text=mensagem, parse_mode="HTML", reply_markup=botoes)
        else:
            bot.send_message(chat_id=chat_id, text=mensagem, parse_mode="HTML")
        time.sleep(2)  # Atraso para evitar limite de taxa
    except TelegramError as e:
        print(f"Erro ao enviar mensagem para {chat_id}: {e}")

# Função para gerar o próximo horário de 3 em 3 minutos
def gerar_proximo_horario(intervalo=3):
    return (datetime.now() + timedelta(minutes=intervalo)).strftime("%H:%M")

# Função para enviar um único horário
def enviar_horario(horario):
    mensagem = (
        f"<b>⏰ HORÁRIO {horario} CONFIRMADO!</b>\n\n"
        "<b>✅ SAIR NA SEGURANÇA 2.00X</b>\n\n"
        "<b>⚠️ VÁLIDO POR 1 MINUTO 🌹</b>\n\n"
        "<b><a href='https://1wxxlb.com/?open=register&p=tlm2'>1WIN</a></b> "
        "<b><a href='https://go.aff.esportiva.bet/ia6vc6x6'>ESPORTIVABET</a></b>\n\n"
        "<b>🔽 CLIQUE NO BOTÃO ABAIXO</b>"
    )
    for chat_id in CHAT_IDS:
        enviar_mensagem(chat_id, mensagem, "Cadastre-se", "https://1wxxlb.com/?open=register&p=tlm2")

# Função para enviar a mensagem de aviso
def enviar_aviso():
    mensagem = (
        "<b>⏰ AVISO!</b>\n\n"
        "<b>FALTAM 3 MINUTOS PARA O INÍCIO DA LISTA DE SINAIS!</b>\n\n"
        "Prepare-se para acompanhar os horários e siga as instruções para melhores resultados.\n\n"
        "<b><a href='https://1wxxlb.com/?open=register&p=tlm2'>1WIN</a></b> "
        "<b><a href='https://go.aff.esportiva.bet/ia6vc6x6'>ESPORTIVABET</a></b> (Para maiores de +18)"
    )
    botoes = InlineKeyboardMarkup([[InlineKeyboardButton("APRENDA AQUI", url="https://t.me/AviatorRosaa/514965")]])
    
    message_ids = {}
    for chat_id in CHAT_IDS:
        try:
            sent_message = bot.send_message(chat_id=chat_id, text=mensagem, parse_mode="HTML", reply_markup=botoes)
            message_ids[chat_id] = sent_message.message_id
            time.sleep(2)  # Atraso para evitar limite de taxa
        except TelegramError as e:
            print(f"Erro ao enviar aviso para {chat_id}: {e}")
    return message_ids

# Função para enviar a mensagem de finalização
def enviar_finalizacao():
    mensagem = (
        "<b>SINAIS 10X FINALIZADOS</b>\n\n"
        "<b>SINAIS A PARTIR DAS 12:00 14:00  21:00 FIQUEM LIGADOS!</b>\n\n"
        "<b>⬇️ CLIQUE NO BOTÃO ABAIXO</b>\n\n"
        "<b><a href='https://go.perfectpay.com.br/PPU38COS6DG'>SEJA PERFECT</a></b>"
    )
    for chat_id in CHAT_IDS:
        enviar_mensagem(chat_id, mensagem, "SEJA PERFECT", "https://go.perfectpay.com.br/PPU38COS6DG")

# Função principal para agendar notificações
def agendar_notificacoes(quantidade_infinita=False, quantidade_sinais=10):
    count = 0
    while True:
        horario = gerar_proximo_horario()
        aviso_antes = (datetime.strptime(horario, "%H:%M") - timedelta(minutes=2)).strftime("%H:%M")
        horario_antes = (datetime.strptime(horario, "%H:%M") - timedelta(minutes=1)).strftime("%H:%M")

        current_time = datetime.now().strftime("%H:%M")
        
        while current_time != aviso_antes:
            current_time = datetime.now().strftime("%H:%M")
            time.sleep(10)

        message_ids_aviso = enviar_aviso()
        
        while current_time != horario_antes:
            current_time = datetime.now().strftime("%H:%M")
            time.sleep(10)
        
        for chat_id, message_id in message_ids_aviso.items():
            try:
                bot.delete_message(chat_id=chat_id, message_id=message_id)
            except TelegramError as e:
                print(f"Erro ao deletar mensagem de aviso no grupo {chat_id}: {e}")

        enviar_horario(horario)
        
        count += 1
        if not quantidade_infinita and count >= quantidade_sinais:
            enviar_finalizacao()
            break

        time.sleep(60)

# Configuração do agendamento
agendar_notificacoes(quantidade_infinita=False, quantidade_sinais=10)
