from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Updater, CommandHandler, CallbackQueryHandler, CallbackContext

# Счетчик кликов
click_count = 0

def start(update: Update, context: CallbackContext) -> None:
    keyboard = [[InlineKeyboardButton("Клик!", callback_data='click')]]
    reply_markup = InlineKeyboardMarkup(keyboard)
    update.message.reply_text('Нажмите на кнопку!', reply_markup=reply_markup)

def button(update: Update, context: CallbackContext) -> None:
    global click_count
    click_count += 1
    query = update.callback_query
    query.answer()
    query.edit_message_text(text=f"Вы нажали на кнопку {click_count} раз!")

def main() -> None:
    # Замените 'YOUR_TOKEN' на ваш токен
    updater = Updater("YOUR_TOKEN")
    dispatcher = updater.dispatcher

    dispatcher.add_handler(CommandHandler("start", start))
    dispatcher.add_handler(CallbackQueryHandler(button))

    updater.start_polling()
    updater.idle()

if name == '__main__':
    main()
