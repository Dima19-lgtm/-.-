<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>СберБизнес.Старт - Интерфейс</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        /* Базовые стили для тела документа */
        body {
            font-family: 'Inter', sans-serif; /* Используем Inter, как современный шрифт без засечек */
            scroll-behavior: smooth;
        }

        /* Стили для основного контента, управляющие переключением страниц */
        .page-content {
            display: none; /* Скрыто по умолчанию */
        }
        .page-content.active {
            display: block; /* Отображается, когда активно */
        }

        /* Стили для контейнера графиков (если используются) */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px; /* Максимальная ширина для графиков */
            margin-left: auto;
            margin-right: auto;
            height: 250px; /* Базовая высота для графиков */
            max-height: 300px; /* Максимальная высота для графиков */
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 300px;
                max-height: 350px;
            }
        }

        /* Стили для карточек */
        .card {
            background-color: white;
            border-radius: 0.75rem; /* Закругленные углы */
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06); /* Тень в стиле Material Design */
            padding: 1.5rem;
            margin-bottom: 1.5rem;
        }

        /* Стили для больших иконок */
        .icon-large {
            font-size: 2.5rem;
            line-height: 1;
        }

        /* Стили для шагов блок-схемы (если используются) */
        .flowchart-step {
            border: 2px solid;
            padding: 0.75rem;
            text-align: center;
            border-radius: 0.5rem;
            margin-bottom: 0.75rem;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.9rem;
        }
        .flowchart-arrow {
            font-size: 1.5rem;
            text-align: center;
            margin-bottom: 0.5rem;
            color: #6C757D; /* Серый цвет для стрелок */
        }

        /* Стили для модальных окон */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background-color: white;
            padding: 2rem;
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            max-width: 500px;
            width: 90%;
            text-align: center;
        }

        /* Обновленные цвета Сбера */
        .sber-green-bg {
            background-color: #21A038; /* Зеленый Сбер */
        }
        .sber-green-text {
            color: #21A038;
        }
        .sber-malachite-bg {
            background-color: #107F8C; /* Малахитовый */
        }
        .sber-malachite-text {
            color: #107F8C;
        }
        .sber-sea-wave-bg {
            background-color: #21A19A; /* Морской волны */
        }
        .sber-sea-wave-text {
            color: #21A19A;
        }
        .sber-clover-bg {
            background-color: #31C2A7; /* Клеверный */
        }
        .sber-clover-text {
            color: #31C2A7;
        }
        .btn-primary {
            @apply sber-green-bg text-white font-semibold py-2 px-4 rounded-lg hover:opacity-90 transition duration-200;
        }
        .btn-secondary {
            @apply sber-malachite-bg text-white font-semibold py-2 px-4 rounded-lg hover:opacity-90 transition duration-200;
        }

        /* Стили для AI-ассистента */
        .ai-assistant-button {
            position: fixed;
            bottom: 1.5rem;
            right: 1.5rem;
            background-color: #21A038; /* Зеленый Сбер */
            color: white;
            border-radius: 9999px; /* Полностью круглый */
            width: 64px; /* Размер кнопки */
            height: 64px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            cursor: pointer;
            transition: background-color 0.2s ease-in-out;
            z-index: 999; /* Поверх других элементов */
        }
        .ai-assistant-button:hover {
            background-color: #1a8f30; /* Чуть темнее при наведении */
        }

        .ai-chat-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .ai-chat-window {
            background-color: white;
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            width: 90%;
            max-width: 450px; /* Максимальная ширина чата */
            height: 70%;
            max-height: 600px; /* Максимальная высота чата */
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .chat-header {
            background-color: #107F8C; /* Малахитовый */
            color: white;
            padding: 1rem 1.5rem;
            font-weight: 600;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-top-left-radius: 0.75rem;
            border-top-right-radius: 0.75rem;
        }

        .chat-messages {
            flex-grow: 1;
            padding: 1rem 1.5rem;
            overflow-y: auto;
            background-color: #F8F9FA; /* Светло-серый фон для сообщений */
        }

        .message-bubble {
            max-width: 80%;
            padding: 0.75rem 1rem;
            border-radius: 0.75rem;
            margin-bottom: 0.75rem;
            line-height: 1.4;
        }

        .message-user {
            background-color: #E9ECEF; /* Очень светло-серый */
            align-self: flex-end;
            margin-left: auto;
            color: #212529;
        }

        .message-ai {
            background-color: #D4EDDA; /* Светлый зеленый */
            align-self: flex-start;
            margin-right: auto;
            color: #212529;
        }

        .chat-input-area {
            padding: 1rem 1.5rem;
            border-top: 1px solid #CED4DA; /* Светло-серый разделитель */
            display: flex;
            align-items: center;
            gap: 0.5rem;
            background-color: white;
        }

        .chat-input-area input {
            flex-grow: 1;
            padding: 0.75rem 1rem;
            border: 1px solid #CED4DA;
            border-radius: 0.5rem;
            outline: none;
            font-size: 0.9rem;
        }

        .chat-input-area button {
            background-color: #21A038; /* Зеленый Сбер */
            color: white;
            padding: 0.75rem 1rem;
            border-radius: 0.5rem;
            font-weight: 500;
            transition: background-color 0.2s ease-in-out;
        }
        .chat-input-area button:hover {
            background-color: #1a8f30;
        }

        /* Градиентные стили */
        .gradient-green-blue {
            background: linear-gradient(to right, #21A038, #107F8C); /* От зеленого к малахитовому */
            color: white;
        }
        .gradient-green-lightgreen {
            background: linear-gradient(to right, #21A038, #31C2A7); /* От зеленого к клеверному */
            color: white;
        }
        .gradient-blue-darkblue {
            background: linear-gradient(to right, #107F8C, #0A5F6C); /* От малахитового к более темному синему */
            color: white;
        }
        .loading-dots span {
            animation: blink 1.4s infinite linear;
        }
        .loading-dots span:nth-child(2) {
            animation-delay: 0.2s;
        }
        .loading-dots span:nth-child(3) {
            animation-delay: 0.4s;
        }
        @keyframes blink {
            0%, 100% { opacity: 0.2; }
            50% { opacity: 1; }
        }
    </style>
</head>
<body class="bg-[#F8F9FA] text-[#212529] flex min-h-screen">

    <aside class="w-64 bg-white shadow-lg p-6 flex-col justify-between hidden md:flex">
        <div>
            <div class="text-2xl font-bold text-[#21A038] mb-8">СберБизнес.Старт</div>
            <nav>
                <ul>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="dashboard">
                            <span class="icon-large">🏠</span>
                            <span>Мой Бизнес</span>
                        </button>
                    </li>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="payments">
                            <span class="icon-large">💸</span>
                            <span>Платежи</span>
                        </button>
                    </li>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="accounts">
                            <span class="icon-large">💳</span>
                            <span>Счета</span>
                        </button>
                    </li>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="products">
                            <span class="icon-large">📦</span>
                            <span>Продукты и Сервисы</span>
                        </button>
                    </li>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="support">
                            <span class="icon-large">❓</span>
                            <span>Поддержка</span>
                        </button>
                    </li>
                    <li class="mb-4">
                        <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="sberguide">
                            <span class="icon-large">📚</span> <span>СберГайд</span>
                        </button>
                    </li>
                </ul>
            </nav>
        </div>
        <div class="mt-8">
            <button class="nav-link w-full py-2 px-4 rounded-lg flex items-center space-x-3 text-[#6C757D] hover:bg-[#E9ECEF] transition-colors duration-200 font-medium" data-page="settings">
                <span class="icon-large">⚙️</span>
                <span>Настройки</span>
            </button>
        </div>
    </aside>

    <main class="flex-1 p-4 md:p-6 flex flex-col">
        <header class="bg-white shadow-md p-4 flex justify-between items-center md:hidden mb-6 rounded-lg">
            <div class="text-xl font-bold text-[#21A038]">СберБизнес.Старт</div>
            <button id="mobile-menu-button" class="text-[#212529]">
                <span class="icon-large">☰</span>
            </button>
        </header>

        <div id="mobile-nav-overlay" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden md:hidden">
            <div class="bg-white w-64 h-full p-6 flex flex-col justify-between">
                <div>
                    <div class="text-2xl font-bold text-[#21A038] mb-8">СберБизнес.Старт</div>
                    <nav>
                        <ul>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="dashboard">
                                    <span class="icon-large">🏠</span>
                                    <span>Мой Бизнес</span>
                                </button>
                            </li>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="payments">
                                    <span class="icon-large">💸</span>
                                    <span>Платежи</span>
                                </button>
                            </li>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="accounts">
                                    <span class="icon-large">💳</span>
                                    <span>Счета</span>
                                </button>
                            </li>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="products">
                                    <span class="icon-large">📦</span>
                                    <span>Продукты и Сервисы</span>
                                </button>
                            </li>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="support">
                                    <span class="icon-large">❓</span>
                                    <span>Поддержка</span>
                                </button>
                            </li>
                            <li class="mb-4">
                                <button class="nav-link w-full text-left py-2 px-4 rounded-lg flex items-center space-x-3 hover:bg-[#E9ECEF] transition-colors duration-200 text-[#212529] font-medium" data-page="sberguide">
                                    <span class="icon-large">📚</span> <span>СберГайд</span>
                                </button>
                            </li>
                        </ul>
                    </nav>
                </div>
                <div class="mt-8">
                    <button class="nav-link w-full py-2 px-4 rounded-lg flex items-center space-x-3 text-[#6C757D] hover:bg-[#E9ECEF] transition-colors duration-200 font-medium" data-page="settings">
                        <span class="icon-large">⚙️</span>
                        <span>Настройки</span>
                    </button>
                </div>
            </div>
        </div>

        <section id="dashboard-page" class="page-content active">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Мой Бизнес</h1>
            
            <div class="flex flex-col md:flex-row items-start md:items-center justify-between mb-8">
                <p class="text-base md:text-lg text-[#6C757D] mb-4 md:mb-0 md:mr-6 flex-grow leading-relaxed">Ваш персонализированный обзор финансов и быстрый доступ к основным операциям.</p>
            </div>
            
            <div class="flex items-center justify-between p-4 rounded-lg shadow-md mb-6 gradient-green-blue">
                <div>
                    <p class="text-white text-lg">На рублёвых счетах, 18:38</p>
                    <p class="text-2xl md:text-3xl font-bold text-white">30 478,62 ₽</p>
                </div>
                <button class="bg-white text-[#21A038] font-semibold py-2 px-4 rounded-lg hover:opacity-90 transition duration-200 text-base">
                    <span class="icon">➕</span>
                    Создать
                </button>
            </div>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3 md:gap-6">
                <div class="card md:col-span-2">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Обзор счетов</h2>
                    <div class="flex flex-col md:flex-row justify-between items-center mb-6">
                        <div>
                            <p class="text-gray-600 text-lg">Текущий баланс:</p>
                            <p class="text-2xl md:text-3xl font-bold sber-green-text">1 250 000 ₽</p>
                        </div>
                        <div class="mt-4 md:mt-0">
                            <p class="text-gray-600 text-lg">Доходы за месяц:</p>
                            <p class="text-xl md:text-2xl font-semibold sber-clover-text">+ 350 000 ₽</p>
                        </div>
                        <div class="mt-4 md:mt-0">
                            <p class="text-gray-600 text-lg">Расходы за месяц:</p>
                            <p class="text-xl md:text-2xl font-semibold text-red-600">- 100 000 ₽</p>
                        </div>
                    </div>
                    <div class="mb-6">
                        <p class="text-gray-600 text-lg">Прогноз остатка на конец месяца:</p>
                        <p class="text-xl md:text-2xl font-semibold sber-malachite-text">1 500 000 ₽</p>
                    </div>
                    <h3 class="text-xl font-semibold text-[#212529] mb-4">Быстрые действия</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                        <button class="btn-primary text-base">
                            <span class="icon">💰</span>
                            Оплатить счет
                        </button>
                        <button class="btn-primary text-base">
                            <span class="icon">🔄</span>
                            Перевод
                        </button>
                        <button class="btn-primary text-base">
                            <span class="icon">🧾</span>
                            Выставить счет
                        </button>
                    </div>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Проактивная аналитика</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Предупреждения о рисках и рекомендации для улучшения финансового состояния.</p>
                    <div class="space-y-3">
                        <div class="bg-red-100 p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-red-600 text-xl">❗</span>
                            <div>
                                <p class="font-medium text-red-700 text-base">Возможен кассовый разрыв 12 июня</p>
                                <p class="text-sm text-red-600">Прогнозируется отрицательный баланс.</p>
                            </div>
                        </div>
                        <div class="bg-yellow-100 p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-yellow-600 text-xl">⚠️</span>
                            <div>
                                <p class="font-medium text-yellow-700 text-base">Задержка платежей от клиента "X"</p>
                                <p class="text-sm text-yellow-600">Проверьте статус задолженности.</p>
                            </div>
                        </div>
                        <div class="bg-blue-100 p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-blue-600 text-xl">💡</span>
                            <div>
                                <p class="font-medium text-blue-700 text-base">Рекомендация: Оптимизируйте расходы на логистику</p>
                                <p class="text-sm text-blue-600">Ваш средний чек выше на 18% по рынку.</p>
                            </div>
                        </div>
                    </div>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base mt-4">Подробнее об аналитике</button>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Мои расходы и бюджет</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Автоматическая категоризация транзакций и контроль бюджета.</p>
                    <div class="space-y-3 mb-4">
                        <div class="flex justify-between items-center bg-[#F8F9FA] p-3 rounded-lg">
                            <div>
                                <p class="text-sm text-[#6C757D]">Категория: Реклама</p>
                                <p class="font-bold text-lg text-[#212529]">25 000 ₽ <span class="text-red-500"> (120% бюджета)</span></p>
                            </div>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Изменить</button>
                        </div>
                        <div class="flex justify-between items-center bg-[#F8F9FA] p-3 rounded-lg">
                            <div>
                                <p class="text-sm text-[#6C757D]">Категория: Аренда</p>
                                <p class="font-bold text-lg text-[#212529]">50 000 ₽ <span class="text-green-500"> (80% бюджета)</span></p>
                            </div>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Изменить</button>
                        </div>
                    </div>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Подробная аналитика расходов</button>
                </div>
            </div>

            <div class="card bg-[#E9ECEF] p-4 rounded-lg shadow-md flex items-center justify-between mt-6 mb-8">
                <div class="flex items-center space-x-3">
                    <span class="icon-large text-[#21A038]">🔔</span>
                    <div>
                        <p class="text-lg font-semibold text-[#212529]">Доступна новая информация</p>
                        <p class="text-sm text-[#6C757D]">Ознакомьтесь с последними обновлениями и новостями.</p>
                    </div>
                </div>
                <button class="btn-primary text-sm">Новое</button>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-6 mb-8">
                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Мой розничный бизнес</h2>
                    <p class="text-base text-[#6C757D] mb-4">Управляйте вашим розничным бизнесом эффективно.</p>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти к разделу</button>
                </div>
                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Мои счета</h2>
                    <p class="text-base text-[#6C757D] mb-4">Просматривайте и управляйте всеми вашими счетами.</p>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти к разделу</button>
                </div>
            </div>

            <div class="card md:col-span-full">
                <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Последние операции</h2>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-[#CED4DA]">
                        <thead class="bg-[#E9ECEF]">
                            <tr>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Дата
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Описание
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Сумма
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Статус
                                </th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-[#CED4DA]">
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">20.05.2025</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">Оплата поставщику "Альфа"</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600">- 50 000 ₽</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm sber-green-text">Исполнен</td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">19.05.2025</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">Поступление от клиента "Бета"</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm sber-green-text">+ 150 000 ₽</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm sber-green-text">Исполнен</td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">18.05.2025</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm">Выплата зарплаты</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600">- 30 000 ₽</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm sber-green-text">Исполнен</td>
                            </tr>
                            </tbody>
                    </table>
                </div>
                <div class="mt-4 text-right">
                    <button class="sber-malachite-text font-medium hover:underline text-sm">Показать все операции</button>
                </div>
            </div>

            <footer class="bg-[#212529] text-white py-8 px-4 md:px-6 mt-8 rounded-lg">
                <div class="max-w-7xl mx-auto grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-8">
                    <div>
                        <h3 class="text-lg font-semibold mb-4">Сервисы</h3>
                        <ul class="space-y-2 text-sm">
                            <li><a href="#" class="hover:underline">Сервисы для людей с особыми потребностями</a></li>
                            <li><a href="#" class="hover:underline">Сервис выставления счетов</a></li>
                            <li><a href="#" class="hover:underline">Размещение денежных средств</a></li>
                        </ul>
                    </div>

                    <div>
                        <h3 class="text-lg font-semibold mb-4">О банке и контакты</h3>
                        <ul class="space-y-2 text-sm mb-4">
                            <li><a href="#" class="hover:underline">Частным клиентам</a></li>
                            <li><a href="#" class="hover:underline">О банке</a></li>
                            <li><a href="#" class="hover:underline">Пресс-центр</a></li>
                            <li><a href="#" class="hover:underline">Закупки</a></li>
                            <li><a href="#" class="hover:underline">Инсайдерам банка</a></li>
                            <li><a href="#" class="hover:underline">Меры безопасности</a></li>
                            <li><a href="#" class="hover:underline">Вакансии</a></li>
                            <li><a href="#" class="hover:underline">Связаться с банком</a></li>
                            <li><a href="#" class="hover:underline">Политика обработки данных</a></li>
                        </ul>
                        <div class="text-sm">
                            <p class="mb-1">Бесплатные звонки с моб. телефонов (Билайн, Мегафон, МТС, СберМобайл, Tele2, Yota) на территории РФ</p>
                            <p class="font-bold text-xl mb-1">0321</p>
                            <p class="font-bold text-xl mb-1">+7 495 665-5-777</p>
                            <p class="text-xs">Звонки из-за рубежа (оплата согласно тарифу)</p>
                        </div>
                    </div>

                    <div>
                        <h3 class="text-lg font-semibold mb-4">Юридическая информация</h3>
                        <p class="text-sm mb-2">Россия, Москва, 117312, ул. Вавилова, 19</p>
                        <p class="text-sm mb-2">© 1997-2025 ПАО Сбербанк.</p>
                        <p class="text-sm mb-2">Генеральная лицензия на осуществление банковских операций от 11 августа 2015 года. Регистрационный номер 1481.</p>
                        <ul class="space-y-2 text-sm">
                            <li><a href="#" class="hover:underline">Информация о процентных ставках по договорам банковского вклада с физическими лицами</a></li>
                            <li><a href="#" class="hover:underline">Информация, обязательная к размещению</a></li>
                            <li><a href="#" class="hover:underline">Раскрытие информации о банке как о профессиональном участнике рынка ценных бумаг</a></li>
                            <li><a href="#" class="hover:underline">На информационном ресурсе применяются рекомендательные технологии</a></li>
                        </ul>
                    </div>

                    <div class="lg:col-span-1 md:col-span-3">
                        <h3 class="text-lg font-semibold mb-4">Язык</h3>
                        <ul class="space-y-2 text-sm">
                            <li><a href="#" class="hover:underline">English</a></li>
                            <li><a href="#" class="hover:underline">中文版</a></li>
                        </ul>
                    </div>
                </div>
            </footer>
        </section>

        <section id="payments-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Платежи</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Удобное создание и отслеживание всех ваших платежей.</p>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 md:gap-6">
                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Создать новый платеж</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Быстро выполните платеж на расчётный счёт компании-контрагента.</p>
                    <div class="space-y-4">
                        <div>
                            <label for="inn" class="block text-sm font-medium text-[#212529] mb-1">ИНН или название компании</label>
                            <input type="text" id="inn" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Введите ИНН или название">
                            <p class="text-xs text-[#6C757D] mt-1">Дождитесь автозаполнения реквизитов (БИК, номер счёта).</p>
                        </div>
                        <div>
                            <label for="amount" class="block text-sm font-medium text-[#212529] mb-1">Сумма</label>
                            <input type="number" id="amount" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Например, 10000">
                        </div>
                        <div>
                            <label for="purpose" class="block text-sm font-medium text-[#212529] mb-1">Назначение платежа</label>
                            <textarea id="purpose" rows="3" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Например, Оплата за услуги по договору №123"></textarea>
                            <p class="text-xs text-[#6C757D] mt-1">Постарайтесь максимально полно описать назначение платежа.</p>
                        </div>
                        <div>
                            <label for="account_from" class="block text-sm font-medium text-[#212529] mb-1">Счет списания</label>
                            <select id="account_from" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>Расчетный счет (₽)</option>
                                <option>Валютный счет ($)</option>
                            </select>
                        </div>
                        <div class="flex items-center">
                            <input type="checkbox" id="save_template" class="h-4 w-4 text-[#21A038] rounded border-[#CED4DA] focus:ring-[#21A038]">
                            <label for="save_template" class="ml-2 text-sm text-[#212529]">Сохранить как шаблон (Совет: сохраняйте часто повторяющиеся платежи как шаблоны — это сократит время на ввод реквизитов.)</label>
                        </div>
                        <button id="submit-payment-button" class="w-full py-3 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Подтвердить и отправить</button>
                    </div>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Мои шаблоны платежей</h2>
                    <div class="space-y-3 mb-6">
                        <div class="p-3 bg-[#F8F9FA] rounded-md flex justify-between items-center text-base">
                            <span>Оплата аренды (ООО "СтройМастер")</span>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Использовать</button>
                        </div>
                        <div class="p-3 bg-[#F8F9FA] rounded-md flex justify-between items-center text-base">
                            <span>Зарплата (Иванов И.И.)</span>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Использовать</button>
                        </div>
                    </div>
                    <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base">Создать новый шаблон</button>

                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mt-8 mb-4">История платежей</h2>
                    <div class="overflow-x-auto">
                        <table class="min-w-full bg-white rounded-lg">
                            <thead>
                                <tr class="bg-[#E9ECEF] text-left text-sm text-[#6C757D] font-medium">
                                    <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Дата
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Получатель
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Сумма
                                </th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-[#6C757D] uppercase tracking-wider">
                                    Статус
                                </th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="border-b border-[#E9ECEF]">
                                <td class="py-3 px-4 text-sm">20.05.2025</td>
                                <td class="py-3 px-4 text-sm">ООО "Поставщик"</td>
                                <td class="py-3 px-4 text-sm text-red-600">- 50 000 ₽</td>
                                <td class="py-3 px-4 text-sm sber-green-text">Исполнен</td>
                            </tr>
                            <tr class="border-b border-[#E9ECEF]">
                                <td class="py-3 px-4 text-sm">17.05.2025</td>
                                <td class="py-3 px-4 text-sm">ФНС (НДС)</td>
                                <td class="py-3 px-4 text-sm text-red-600">- 30 000 ₽</td>
                                <td class="py-3 px-4 text-sm sber-malachite-text">В обработке</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                <button class="mt-4 sber-malachite-text hover:underline text-sm font-medium">Посмотреть всю историю</button>
            </div>
        </section>

        <section id="accounts-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Счета</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Управляйте всеми вашими счетами и получайте выписки.</p>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 md:gap-6">
                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Мои активные счета</h2>
                    <div class="space-y-4">
                        <div class="p-4 bg-[#F8F9FA] rounded-md border border-[#CED4DA]">
                            <div class="flex justify-between items-center mb-2">
                                <span class="font-bold text-lg">Расчетный счет (₽)</span>
                                <span class="sber-green-text font-bold text-lg">1 100 000 ₽</span>
                            </div>
                            <p class="text-sm text-[#6C757D]">Открыт: 12.01.2024</p>
                            <button class="mt-3 sber-malachite-text hover:underline text-sm font-medium">Запросить выписку</button>
                        </div>
                        <div class="p-4 bg-[#F8F9FA] rounded-md border border-[#CED4DA]">
                            <div class="flex justify-between items-center mb-2">
                                <span class="font-bold text-lg">Валютный счет ($)</span>
                                <span class="sber-green-text font-bold text-lg">1 500 $</span>
                            </div>
                            <p class="text-sm text-[#6C757D]">Открыт: 01.03.2024</p>
                            <button class="mt-3 sber-malachite-text hover:underline text-sm font-medium">Запросить выписку</button>
                        </div>
                    </div>
                    <button class="mt-6 w-full py-3 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Открыть новый счет</button>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">СберКопилка для Налогов</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Автоматическое резервирование средств для уплаты налогов.</p>
                    <div class="space-y-3 mb-4">
                        <div class="flex justify-between items-center bg-[#F8F9FA] p-3 rounded-lg">
                            <div>
                                <p class="text-sm text-[#6C757D]">Процент резервирования</p>
                                <p class="font-bold text-lg text-[#212529]">6% от доходов</p>
                            </div>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Настроить</button>
                        </div>
                        <div class="flex justify-between items-center bg-[#F8F9FA] p-3 rounded-lg">
                            <div>
                                <p class="text-sm text-[#6C757D]">Накоплено для налогов</p>
                                <p class="font-bold text-lg text-[#21A038]">12 345 ₽</p>
                            </div>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">История</button>
                        </div>
                        <div class="flex justify-between items-center bg-[#F8F9FA] p-3 rounded-lg">
                            <div>
                                <p class="text-sm text-[#6C757D]">Прогноз налога к уплате (до 25.06)</p>
                                <p class="font-bold text-lg text-[#212529]">15 000 ₽</p>
                            </div>
                            <span class="icon text-yellow-600 text-xl">🔔</span>
                        </div>
                    </div>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти к СберКопилке</button>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Получение выписки по счёту</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Сформируйте и скачайте выписку по движению средств.</p>
                    <div class="space-y-4">
                        <div>
                            <label for="statement_account" class="block text-sm font-medium text-[#212529] mb-1">Выберите расчетный счет</label>
                            <select id="statement_account" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>Расчетный счет (₽)</option>
                                <option>Валютный счет ($)</option>
                            </select>
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <div>
                                <label for="start_date" class="block text-sm font-medium text-[#212529] mb-1">Дата начала</label>
                                <input type="date" id="start_date" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                            </div>
                            <div>
                                <label for="end_date" class="block text-sm font-medium text-[#212529] mb-1">Дата окончания</label>
                                <input type="date" id="end_date" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                            </div>
                        </div>
                        <div>
                            <label for="file_format" class="block text-sm font-medium text-[#212529] mb-1">Формат файла</label>
                            <select id="file_format" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>PDF</option>
                                <option>CSV</option>
                            </select>
                        </div>
                        <button class="w-full py-3 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Сформировать</button>
                    </div>
                    <h3 class="text-lg font-semibold text-[#212529] mt-6 mb-3">Готовые выписки</h3>
                    <div class="space-y-3">
                        <div class="p-3 bg-[#F8F9FA] rounded-md flex justify-between items-center text-base">
                            <span>Выписка за Апрель 2025</span>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Скачать PDF</button>
                        </div>
                        <div class="p-3 bg-[#F8F9FA] rounded-md flex justify-between items-center text-base">
                            <span>Выписка за Март 2025</span>
                            <button class="sber-malachite-text hover:underline text-sm font-medium">Скачать PDF</button>
                        </div>
                    </div>
                    <p class="text-xs text-[#6C757D] mt-4">Совет: регулярно скачивайте ключевые отчёты для аналитики.</p>
                </div>
            </div>
        </section>

        <section id="products-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Продукты и Сервисы</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Актуальные предложения Сбера для роста вашего бизнеса.</p>

            <div class="card mb-6">
                <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Курсы валют</h2>
                <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-4 text-center">
                    <div class="bg-[#F8F9FA] p-3 rounded-lg">
                        <p class="font-bold text-lg text-[#212529]">USD</p>
                        <p class="text-sm text-[#6C757D]">86,45 ▲</p>
                    </div>
                    <div class="bg-[#F8F9FA] p-3 rounded-lg">
                        <p class="font-bold text-lg text-[#212529]">EUR</p>
                        <p class="text-sm text-[#6C757D]">95,89 ▲</p>
                    </div>
                    <div class="bg-[#F8F9FA] p-3 rounded-lg">
                        <p class="font-bold text-lg text-[#212529]">CNY</p>
                        <p class="text-sm text-[#6C757D]">11,55 ▲</p>
                    </div>
                    <div class="bg-[#F8F9FA] p-3 rounded-lg">
                        <p class="font-bold text-lg text-[#212529]">INR</p>
                        <p class="text-sm text-[#6C757D]">98,49 ▲</p>
                    </div>
                    <div class="bg-[#F8F9FA] p-3 rounded-lg">
                        <p class="font-bold text-lg text-[#21257D]">Au</p>
                        <p class="text-sm text-[#6C757D]">8 640,00 ▲</p>
                    </div>
                </div>
                <div class="mt-4 text-right">
                    <button class="sber-malachite-text font-medium hover:underline text-sm">Настроить виджеты</button>
                </div>
            </div>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 lg:grid-cols-3 md:gap-6">
                <div class="card">
                    <div class="sber-green-text icon-large mb-3">💰</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Расчетно-кассовое обслуживание</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Выберите тариф, который идеально подходит для вашего стартапа. Открытие счета за 5 минут.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4">
                        <li>Тариф "Легкий старт"</li>
                        <li>Бесплатное обслуживание первые 3 месяца</li>
                    </ul>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Оформить заявку</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">📈</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Оформление кредита</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Выберите кредитный продукт, рассчитайте график и отправьте заявку.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Отфильтруйте предложения по сумме, сроку и цели.</li>
                        <li>В калькуляторе установите сумму и срок.</li>
                        <li>Заполните онлайн-анкету (сведения о компании, финансы, цель).</li>
                        <li>Прикрепите необходимые документы в PDF.</li>
                    </ul>
                    <p class="text-xs text-[#6C757D] mb-4">Совет: заранее подготовьте документы, чтобы ускорить подачу.</p>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Оформить заявку</button>
                    <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base mt-2">Мои заявки</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">🚚</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Заказ самоинкассации</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Закажите выезд курьера-инкассатора для забора наличных.</p>
                    <div class="space-y-3 mb-4">
                        <div>
                            <label for="pickup_address" class="block text-sm font-medium text-[#212529] mb-1">Адрес забора</label>
                            <input type="text" id="pickup_address" class="w-full p-2 border border-[#CED4DA] rounded-lg text-base" placeholder="Выберите сохраненный или введите новый">
                        </div>
                        <div>
                            <label for="cash_amount" class="block text-sm font-medium text-[#212529] mb-1">Сумма наличных и валюта</label>
                            <input type="text" id="cash_amount" class="w-full p-2 border border-[#CED4DA] rounded-lg text-base" placeholder="Например, 100 000 ₽">
                        </div>
                        <div>
                            <label for="pickup_datetime" class="block text-sm font-medium text-[#212529] mb-1">Дата и время приезда курьера</label>
                            <input type="datetime-local" id="pickup_datetime" class="w-full p-2 border border-[#CED4DA] rounded-lg text-base">
                        </div>
                    </div>
                    <p class="text-xs text-[#6C757D] mb-4">Совет: планируйте заказы заранее, чтобы выбрать удобное время.</p>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Заказать инкассацию</button>
                    <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base mt-2">История заказов</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">💳</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Бизнес-карты</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Удобные расчеты, снятие наличных и контроль расходов для вашего бизнеса.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4">
                        <li>Бесплатное обслуживание</li>
                        <li>Кэшбэк на бизнес-расходы</li>
                    </ul>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Заказать карту</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">🛒</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Эквайринг</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Принимайте платежи от клиентов любым удобным способом: онлайн, по QR-коду или через терминал.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4">
                        <li>Низкие комиссии</li>
                        <li>Быстрое подключение</li>
                    </ul>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Подключить</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">📊</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Онлайн-бухгалтерия</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Упростите ведение учета и сдачу отчетности с нашими партнерскими сервисами.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4">
                        <li>Автоматический расчет налогов</li>
                        <li>Формирование деклараций</li>
                    </ul>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Узнать подробнее</button>
                </div>

                <div class="card md:col-span-full">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Экосистема и Маркетплейс</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Единое окно управления бизнесом: от платежей до бухгалтерии и маркетинга.</p>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 mb-4">
                        <div class="bg-[#F8F9FA] p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-green-600 text-xl">🔗</span>
                            <div>
                                <p class="font-medium text-[#212529] text-base">Интеграция с 1С-Бухгалтерией</p>
                                <p class="text-sm text-[#6C757D]">Автоэкспорт платежей, импорт счетов.</p>
                            </div>
                        </div>
                        <div class="bg-[#F8F9FA] p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-blue-600 text-xl">🔗</span>
                            <div>
                                <p class="font-medium text-[#212529] text-base">Яндекс Бизнес / Метрика</p>
                                <p class="text-sm text-[#6C757D]">Таргет, реклама, аналитика по лидам.</p>
                            </div>
                        </div>
                        <div class="bg-[#F8F9FA] p-3 rounded-lg flex items-center space-x-3">
                            <span class="icon text-purple-600 text-xl">🔗</span>
                            <div>
                                <p class="font-medium text-[#212529] text-base">CRM-системы (amoCRM, Bitrix24)</p>
                                <p class="text-sm text-[#6C757D]">Интеграция для обработки заявок.</p>
                            </div>
                        </div>
                    </div>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти в Маркетплейс</button>
                </div>
            </div>
        </section>

        <section id="support-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Поддержка</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Мы всегда готовы помочь вам с любыми вопросами.</p>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 md:gap-6">
                <div class="card">
                    <div class="sber-green-text icon-large mb-3">📚</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">База знаний / FAQ</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Найдите ответы на самые частые вопросы, посмотрите видеоуроки и изучите глоссарий.</p>
                    <input type="text" placeholder="Поиск по Базе знаний..." class="w-full p-2 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base mb-4">
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Начало работы</li>
                        <li>Платежи и переводы</li>
                        <li>Налоги и отчетность</li>
                        <li>Популярные вопросы</li>
                    </ul>
                    <p class="text-xs text-[#6C757D] mb-4">Короткие видеоуроки и интерактивные инструкции помогут быстрее разобраться.</p>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти в Базу знаний</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">💬</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Чат с поддержкой</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Получите быструю консультацию от наших специалистов в режиме реального времени.</p>
                    <div class="h-32 bg-[#F8F9FA] rounded-md p-3 mb-4 overflow-y-auto text-sm text-[#6C757D]">
                        <p>Оператор: Здравствуйте! Чем могу помочь?</p>
                        <p>Вы: Добрый день! У меня вопрос по заполнению платежного поручения.</p>
                    </div>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Начать чат</button>
                    <a href="#" class="block text-center sber-malachite-text hover:underline text-sm font-medium mt-2">История чатов</a>
                    <div class="mt-4 space-y-2">
                        <p class="text-sm font-medium text-[#212529]">Частые проблемы:</p>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                            <button class="py-2 px-3 bg-[#E9ECEF] text-[#212529] rounded-lg text-sm hover:bg-[#CED4DA] transition-colors duration-200">Не пришел платеж</button>
                            <button class="py-2 px-3 bg-[#E9ECEF] text-[#212529] rounded-lg text-sm hover:bg-[#CED4DA] transition-colors duration-200">Заблокирован аккаунт</button>
                            <button class="py-2 px-3 bg-[#E9ECEF] text-[#212529] rounded-lg text-sm hover:bg-[#CED4DA] transition-colors duration-200">Не работает сервис</button>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">🤝</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Сообщества и форумы</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Обсуждайте вопросы, делитесь опытом и получайте ответы от других предпринимателей и экспертов Сбера.</p>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Перейти в сообщество</button>
                    <p class="text-xs text-[#6C757D] mt-4">Возможность обсуждения с другими предпринимателями. Ответы от модераторов / экспертов Сбера.</p>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">📝</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Обратная связь</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Ваше мнение помогает нам стать лучше! Оставьте свои предложения или сообщите об ошибке.</p>
                    <textarea rows="4" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Ваше сообщение..."></textarea>
                    <button class="mt-4 w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base">Отправить</button>
                </div>

                <div class="card md:col-span-2">
                    <div class="sber-green-text icon-large mb-3">📞</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Телефонная поддержка</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Свяжитесь с нами по телефону для оперативной консультации.</p>
                    <p class="text-2xl font-bold text-[#212529] mb-2">8 800 555-55-50</p>
                    <p class="text-sm text-[#6C757D] mb-4">График работы: Пн-Пт с 9:00 до 18:00 (МСК)</p>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Заказать обратный звонок</button>
                    <p class="text-xs text-[#6C757D] mt-4">Для VIP-клиентов доступен персональный менеджер.</p>
                </div>
            </div>
        </section>

        <section id="sberguide-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Путеводитель молодого предпринимателя</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Пошаговый гайд по запуску и развитию вашего бизнеса со СберБизнес.Старт.</p>

            <div class="card mb-8">
                <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Ваш прогресс</h2>
                <div class="w-full bg-[#E9ECEF] rounded-full h-4 mb-4">
                    <div class="sber-green-bg h-4 rounded-full" style="width: 40%;"></div>
                </div>
                <p class="text-sm text-[#6C757D] text-center">40% пройдено (2 из 5 модулей)</p>
            </div>

            <div class="card mb-6">
                <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Важное для бизнеса</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="bg-[#F8F9FA] p-4 rounded-lg flex flex-col items-center text-center">
                        <img src="https://placehold.co/60x60/21A038/FFFFFF?text=Юр" alt="Юридическая поддержка" class="mb-2 rounded-full">
                        <h3 class="text-lg font-semibold text-[#212529]">Юридическая поддержка бизнеса</h3>
                        <a href="#" class="text-sm sber-malachite-text hover:underline font-medium">Подробнее</a>
                    </div>
                    <div class="bg-[#F8F9FA] p-4 rounded-lg flex flex-col items-center text-center">
                        <img src="https://placehold.co/60x60/21A038/FFFFFF?text=Отч" alt="Отчетность в госорганы" class="mb-2 rounded-full">
                        <h3 class="text-lg font-semibold text-[#212529]">Отчётность в госорганы</h3>
                        <a href="#" class="text-sm sber-malachite-text hover:underline font-medium">Подробнее</a>
                    </div>
                    <div class="bg-[#F8F9FA] p-4 rounded-lg flex flex-col items-center text-center relative overflow-hidden">
                        <div class="absolute top-0 right-0 bg-red-500 text-white text-xs px-2 py-1 rounded-bl-lg">Новое</div>
                        <img src="https://placehold.co/60x60/21A038/FFFFFF?text=Новости" alt="Новости" class="mb-2 rounded-full">
                        <h3 class="text-lg font-semibold text-[#212529]">Маркировка звонков, 3-НДФЛ без штрафов, рост онлайн-продаж</h3>
                        <a href="#" class="text-sm sber-malachite-text hover:underline font-medium">Новости</a>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-6">
                <div class="card">
                    <div class="sber-green-text icon-large mb-3">🚀</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Модуль 1: С чего начать?</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Основные шаги для регистрации бизнеса и открытия первого счета.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Выбор формы собственности (ИП или ООО)</li>
                        <li>Регистрация бизнеса: пошаговая инструкция</li>
                        <li>Открытие расчетного счета в СберБизнес.Старт</li>
                    </ul>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Начать модуль</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">💸</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Модуль 2: Управление финансами</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Как эффективно управлять доходами и расходами, совершать платежи.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Платежи контрагентам: создание и шаблоны</li>
                        <li>Выставление счетов клиентам</li>
                        <li>Анализ движения средств и отчетность</li>
                    </ul>
                    <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Продолжить модуль</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">⚖️</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Модуль 3: Юридические аспекты и налоги</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Основы налогообложения, отчетности и юридической безопасности.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Выбор системы налогообложения (УСН, ОСНО)</li>
                        <li>Сдача отчетности: сроки и формы</li>
                        <li>Юридическая поддержка для малого бизнеса</li>
                    </ul>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Начать модуль</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">💡</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Модуль 4: Развитие и масштабирование</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Инструменты для роста вашего бизнеса и привлечения клиентов.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Привлечение финансирования: кредиты и гранты</li>
                        <li>Эквайринг и онлайн-кассы</li>
                        <li>Маркетинг и продажи для стартапов</li>
                    </ul>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Начать модуль</button>
                </div>

                <div class="card">
                    <div class="sber-green-text icon-large mb-3">🤝</div>
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-3">Модуль 5: Сообщество и поддержка</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Где найти ответы на вопросы и поддержку от экспертов и коллег.</p>
                    <ul class="list-disc list-inside text-[#6C757D] text-sm mb-4 space-y-1">
                        <li>Чат с поддержкой СберБизнес.Старт</li>
                        <li>База знаний и FAQ</li>
                        <li>Сообщество предпринимателей</li>
                    </ul>
                    <button class="w-full py-2 px-4 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Начать модуль</button>
                </div>
            </div>

            <div class="card mt-6">
                <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Дополнительные ресурсы</h2>
                <ul class="space-y-3">
                    <li>
                        <a href="#" class="flex items-center text-base font-medium sber-sea-wave-text hover:underline">
                            <span class="icon mr-2">🔗</span>
                            Глоссарий терминов для предпринимателей
                        </a>
                    </li>
                    <li>
                        <a href="#" class="flex items-center text-base font-medium sber-sea-wave-text hover:underline">
                            <span class="icon mr-2">📅</span>
                            Календарь важных дат для бизнеса (налоги, отчетность)
                        </a>
                    </li>
                </ul>
            </div>
        </section>

        <section id="settings-page" class="page-content">
            <h1 class="text-2xl md:text-3xl font-bold text-[#212529] mb-6">Настройки</h1>
            <p class="text-base md:text-lg text-[#6C757D] mb-8">Персонализируйте работу приложения под ваши нужды.</p>

            <div class="grid grid-cols-1 gap-4 md:grid-cols-2 md:gap-6">
                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Общие настройки</h2>
                    <div class="space-y-4">
                        <div>
                            <label for="notification_settings" class="block text-sm font-medium text-[#212529] mb-1">Уведомления</label>
                            <select id="notification_settings" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>Все уведомления</option>
                                <option>Только важные</option>
                                <option>Отключить</option>
                            </select>
                        </div>
                        <div>
                            <label for="app_language" class="block text-sm font-medium text-[#212529] mb-1">Язык приложения</label>
                            <select id="app_language" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>Русский</option>
                                <option>English</option>
                            </select>
                        </div>
                        <div class="flex items-center justify-between">
                            <span class="text-sm font-medium text-[#212529]">Темная тема</span>
                            <label class="relative inline-flex items-center cursor-pointer">
                                <input type="checkbox" value="" class="sr-only peer">
                                <div class="w-11 h-6 bg-[#CED4DA] peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-[#21A038] rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-[#21A038]"></div>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Настройки СберКопилки для Налогов</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Автоматическое отчисление процентов от доходов для налогов.</p>
                    <div class="space-y-4">
                        <div>
                            <label for="tax_percentage" class="block text-sm font-medium text-[#212529] mb-1">Процент отчислений от доходов</label>
                            <input type="range" id="tax_percentage" min="1" max="15" value="6" class="w-full h-2 bg-[#E9ECEF] rounded-lg appearance-none cursor-pointer sber-green-bg" oninput="document.getElementById('tax_percentage_value').innerText = this.value + '%'">
                            <span id="tax_percentage_value" class="block text-center text-sm font-medium text-[#212529] mt-2">6%</span>
                        </div>
                        <div>
                            <label for="tax_system" class="block text-sm font-medium text-[#212529] mb-1">Ваша налоговая система</label>
                            <select id="tax_system" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>УСН 6%</option>
                                <option>НПД (Самозанятость)</option>
                                <option>ПСН</option>
                                <option>ОСНО</option>
                            </select>
                        </div>
                        <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Сохранить настройки копилки</button>
                    </div>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Настройки расходов и бюджета</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Управляйте автоматической категоризацией и устанавливайте бюджетные лимиты.</p>
                    <div class="space-y-4">
                        <div>
                            <label for="default_category_rule" class="block text-sm font-medium text-[#212529] mb-1">Правила автокатегоризации</label>
                            <select id="default_category_rule" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>По MCC-коду</option>
                                <option>По ИНН/названию контрагента</option>
                                <option>По историческим данным</option>
                            </select>
                        </div>
                        <div>
                            <label for="budget_limit_category" class="block text-sm font-medium text-[#212529] mb-1">Установить бюджетный лимит для категории</label>
                            <input type="text" id="budget_limit_category" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base mb-2" placeholder="Например, Реклама">
                            <input type="number" id="budget_limit_amount" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Сумма, например, 10000">
                        </div>
                        <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Сохранить настройки бюджета</button>
                        <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base mt-2">Редактировать категории вручную</button>
                    </div>
                </div>

                <div class="card">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Профиль бизнеса и цели</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Расскажите о вашем бизнесе, чтобы получать более персонализированные предложения.</p>
                    <div class="space-y-4">
                        <div>
                            <label for="business_niche" class="block text-sm font-medium text-[#212529] mb-1">Ниша бизнеса</label>
                            <input type="text" id="business_niche" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Например, Розничная торговля, Услуги">
                        </div>
                        <div>
                            <label for="business_turnover" class="block text-sm font-medium text-[#212529] mb-1">Примерный оборот в месяц (₽)</label>
                            <input type="number" id="business_turnover" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Например, 100000">
                        </div>
                        <div>
                            <label for="development_stage" class="block text-sm font-medium text-[#212529] mb-1">Стадия развития бизнеса</label>
                            <select id="development_stage" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base">
                                <option>Стартап (до 1 года)</option>
                                <option>Развивающийся (1-3 года)</option>
                                <option>Стабильный (более 3 лет)</option>
                            </select>
                        </div>
                        <div>
                            <label for="user_goals" class="block text-sm font-medium text-[#212529] mb-1">Мои цели (через запятую)</label>
                            <textarea id="user_goals" rows="3" class="w-full p-3 border border-[#CED4DA] rounded-lg focus:ring-[#21A038] focus:border-[#21A038] text-base" placeholder="Например, Увеличить прибыль, Расширить ассортимент"></textarea>
                        </div>
                        <button class="w-full py-2 px-4 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Сохранить профиль</button>
                    </div>
                </div>

                <div class="card md:col-span-2">
                    <h2 class="text-xl md:text-2xl font-semibold sber-malachite-text mb-4">Безопасность</h2>
                    <p class="text-sm md:text-base text-[#6C757D] mb-4">Управляйте настройками безопасности вашего аккаунта.</p>
                    <div class="space-y-4">
                        <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base">Изменить пароль</button>
                        <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base">Настроить двухфакторную аутентификацию</button>
                        <button class="w-full py-2 px-4 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base">История входов</button>
                    </div>
                </div>
            </div>
        </section>


        <div id="payment-confirm-modal" class="modal-overlay hidden">
            <div class="modal-content">
                <h2 class="text-xl md:text-2xl font-bold sber-malachite-text mb-4">Подтверждение платежа</h2>
                <p class="text-base md:text-lg text-[#212529] mb-4">Пожалуйста, проверьте детали платежа:</p>
                <div class="text-left bg-[#F8F9FA] p-4 rounded-md mb-6 space-y-2 text-sm">
                    <p><strong>Получатель:</strong> <span id="modal-recipient"></span></p>
                    <p><strong>Сумма:</strong> <span id="modal-amount"></span></p>
                    <p><strong>Назначение:</strong> <span id="modal-purpose"></span></p>
                    <p><strong>Со счета:</strong> <span id="modal-account-from"></span></p>
                </div>
                <p class="text-xs md:text-sm text-red-600 mb-4">Шаг 4: Подтвердите операцию кодом из SMS (имитация).</p>
                <div class="flex flex-col sm:flex-row justify-center space-y-3 sm:space-y-0 sm:space-x-4">
                    <button id="confirm-payment-button" class="py-2 px-6 bg-[#21A038] text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Подтвердить</button>
                    <button id="cancel-payment-button" class="py-2 px-6 bg-[#CED4DA] text-[#212529] rounded-lg font-semibold hover:bg-[#ADB5BD] transition-colors duration-200 text-base">Отмена</button>
                </div>
            </div>
        </div>

        <div id="payment-result-modal" class="modal-overlay hidden">
            <div class="modal-content">
                <h2 class="text-xl md:text-2xl font-bold sber-green-text mb-4">Платеж успешно отправлен!</h2>
                <p class="text-base md:text-lg text-[#212529] mb-4">Шаг 5: Дождитесь статуса "Выполнено". Обычно мгновенно или в течение рабочего дня.</p>
                <p class="text-xs md:text-sm text-[#6C757D] mb-6">Вы можете сохранить квитанцию в PDF или экспортировать в бухгалтерию.</p>
                <div class="flex flex-col sm:flex-row justify-center space-y-3 sm:space-y-0 sm:space-x-4">
                    <button class="py-2 px-6 sber-malachite-bg text-white rounded-lg font-semibold hover:bg-opacity-90 transition-colors duration-200 text-base">Скачать квитанцию (PDF)</button>
                    <button class="py-2 px-6 bg-[#E9ECEF] text-[#212529] rounded-lg font-semibold hover:bg-[#CED4DA] transition-colors duration-200 text-base" onclick="showPage('dashboard'); document.getElementById('payment-result-modal').classList.add('hidden');">Вернуться на главную</button>
                </div>
            </div>
        </div>

        <button id="ai-assistant-button" class="ai-assistant-button hidden">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-8 h-8 text-white">
                <text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" font-family="Inter, sans-serif" font-size="12" font-weight="bold" fill="white">AI</text>
            </svg>
        </button>

        <div id="ai-assistant-modal" class="ai-chat-modal hidden">
            <div class="ai-chat-window">
                <div class="chat-header">
                    <span>AI-ассистент ✨</span>
                    <button id="close-ai-chat" class="text-white text-xl leading-none">&times;</button>
                </div>
                <div id="chat-messages" class="chat-messages flex flex-col">
                    <div class="message-bubble message-ai">
                        Здравствуйте! Я ваш AI-ассистент. Чем могу помочь сегодня?
                    </div>
                </div>
                <div class="chat-input-area">
                    <input type="text" id="ai-chat-input" placeholder="Введите ваше сообщение..." />
                    <button id="send-ai-message">Отправить</button>
                </div>
            </div>
        </div>
    </main>

    <script>
        // Обновленные цвета Сбера в соответствии с гайдом
        const primaryColor = '#21A038'; // Зеленый Сбер (HEX: #21A038)
        const secondaryColor = '#107F8C'; // Малахитовый (HEX: #107F8C)
        const seaWaveColor = '#21A19A'; // Морской волны (HEX: #21A19A)
        const cloverColor = '#31C2A7'; // Клеверный (HEX: #31C2A7)
        const darkTextColor = '#212529'; // Темно-серый для основного текста
        const grayColor = '#6C757D'; // Серый для второстепенного текста
        const lightGrayColor = '#CED4DA'; // Светло-серый для границ и фонов элементов
        const veryLightGrayColor = '#E9ECEF'; // Очень светло-серый для фонов таблиц и кнопок

        // Функция для переноса длинных меток для Chart.js (сохранена для совместимости, если графики будут добавлены)
        function wrapLabels(labels, maxLength = 16) {
            return labels.map(label => {
                if (label.length <= maxLength) {
                    return label;
                }
                const words = label.split(' ');
                const lines = [];
                let currentLine = '';
                for (const word of words) {
                    if ((currentLine + word).length > maxLength && currentLine.length > 0) {
                        lines.push(currentLine.trim());
                        currentLine = '';
                    }
                    currentLine += word + ' ';
                }
                if (currentLine.length > 0) {
                    lines.push(currentLine.trim());
                }
                return lines;
            });
        }
        
        // Callback для заголовка подсказки Chart.js (сохранена для совместимости)
        const tooltipTitleCallback = function(tooltipItems) {
            const item = tooltipItems[0];
            let label = item.chart.data.labels[item.dataIndex];
            if (Array.isArray(label)) {
              return label.join(' ');
            } else {
              return label;
            }
        };

        // Функция для отображения конкретной страницы и скрытия остальных
        function showPage(pageId) {
            document.querySelectorAll('.page-content').forEach(page => {
                page.classList.remove('active');
            });
            document.getElementById(pageId + '-page').classList.add('active');

            // Закрыть мобильное меню, если оно открыто
            const mobileNavOverlay = document.getElementById('mobile-nav-overlay');
            if (!mobileNavOverlay.classList.contains('hidden')) {
                mobileNavOverlay.classList.add('hidden');
            }

            // Показать/скрыть кнопку AI-ассистента в зависимости от страницы
            const aiAssistantButton = document.getElementById('ai-assistant-button');
            const aiAssistantModal = document.getElementById('ai-assistant-modal');
            if (pageId === 'dashboard') {
                aiAssistantButton.classList.remove('hidden');
            } else {
                aiAssistantButton.classList.add('hidden');
                aiAssistantModal.classList.add('hidden'); // Скрыть модальное окно, если оно открыто
            }
        }

        // Обработчики событий для навигационных ссылок
        document.querySelectorAll('.nav-link').forEach(button => {
            button.addEventListener('click', () => {
                showPage(button.dataset.page);
            });
        });

        // Переключение мобильного меню
        document.getElementById('mobile-menu-button').addEventListener('click', () => {
            document.getElementById('mobile-nav-overlay').classList.remove('hidden');
        });
        document.getElementById('mobile-nav-overlay').addEventListener('click', (event) => {
            if (event.target === event.currentTarget) { // Закрыть только при клике на сам оверлей, а не на его содержимое
                document.getElementById('mobile-nav-overlay').classList.add('hidden');
            }
        });

        // Логика процесса оплаты
        const submitPaymentButton = document.getElementById('submit-payment-button');
        const paymentConfirmModal = document.getElementById('payment-confirm-modal');
        const confirmPaymentButton = document.getElementById('confirm-payment-button');
        const cancelPaymentButton = document.getElementById('cancel-payment-button');
        const paymentResultModal = document.getElementById('payment-result-modal');

        submitPaymentButton.addEventListener('click', () => {
            const inn = document.getElementById('inn').value;
            const amount = document.getElementById('amount').value;
            const purpose = document.getElementById('purpose').value;
            const accountFrom = document.getElementById('account_from').value;

            if (!inn || !amount || !purpose || !accountFrom) {
                console.log('Пожалуйста, заполните все поля для платежа.');
                return;
            }

            // Заполнение модального окна деталями платежа
            document.getElementById('modal-recipient').textContent = `Поставщик (ИНН: ${inn})`;
            document.getElementById('modal-amount').textContent = `${amount} ₽`;
            document.getElementById('modal-purpose').textContent = purpose;
            document.getElementById('modal-account-from').textContent = accountFrom;

            paymentConfirmModal.classList.remove('hidden');
        });

        confirmPaymentButton.addEventListener('click', () => {
            paymentConfirmModal.classList.add('hidden');
            paymentResultModal.classList.remove('hidden');
            // Здесь можно добавить логику для имитации выполнения платежа и обновления истории
        });

        cancelPaymentButton.addEventListener('click', () => {
            paymentConfirmModal.classList.add('hidden');
        });

        // Логика AI-ассистента
        const aiAssistantButton = document.getElementById('ai-assistant-button');
        const aiAssistantModal = document.getElementById('ai-assistant-modal');
        const closeAiChatButton = document.getElementById('close-ai-chat');
        const chatMessagesContainer = document.getElementById('chat-messages');
        const aiChatInput = document.getElementById('ai-chat-input');
        const sendAiMessageButton = document.getElementById('send-ai-message');

        aiAssistantButton.addEventListener('click', () => {
            aiAssistantModal.classList.remove('hidden');
        });

        closeAiChatButton.addEventListener('click', () => {
            aiAssistantModal.classList.add('hidden');
        });

        aiAssistantModal.addEventListener('click', (event) => {
            if (event.target === event.currentTarget) { // Закрыть только при клике на сам оверлей
                aiAssistantModal.classList.add('hidden');
            }
        });

        sendAiMessageButton.addEventListener('click', sendMessage);
        aiChatInput.addEventListener('keypress', (event) => {
            if (event.key === 'Enter') {
                sendMessage();
            }
        });

        async function sendMessage() {
            const userMessage = aiChatInput.value.trim();
            if (userMessage === '') return;

            addMessage(userMessage, 'user');
            aiChatInput.value = '';

            // Добавляем индикатор загрузки
            const loadingMessageDiv = document.createElement('div');
            loadingMessageDiv.classList.add('message-bubble', 'message-ai', 'loading-dots');
            loadingMessageDiv.innerHTML = '<span>.</span><span>.</span><span>.</span>';
            chatMessagesContainer.appendChild(loadingMessageDiv);
            chatMessagesContainer.scrollTop = chatMessagesContainer.scrollHeight;

            try {
                // Подготовка промпта для Gemini API
                const prompt = `Вы - AI-ассистент для платформы СберБизнес.Старт. Ваша задача - отвечать на вопросы пользователей, связанные с функциями платформы, финансами, налогами и ведением бизнеса. Также вы можете имитировать выполнение командных действий, если пользователь их запрашивает. Если вопрос не относится к этим темам, вежливо сообщите, что вы не можете на него ответить. Вот вопрос пользователя: "${userMessage}"

Примеры ответов:
- Если пользователь спрашивает про налоги: "Для оплаты налогов перейдите в раздел 'Платежи' и выберите 'Оплатить налоги'. Там вы сможете указать необходимые реквизиты."
- Если пользователь спрашивает про кредиты: "В разделе 'Продукты и Сервисы' вы найдете информацию о кредитах для бизнеса. Вы можете оформить заявку онлайн."
- Если пользователь говорит "Оплати счет": "Хорошо, для оплаты счета мне потребуется ИНН получателя, сумма и назначение платежа. Пожалуйста, предоставьте эти данные."
- Если пользователь спрашивает "сколько у меня осталось денег": "На ваших рублёвых счетах сейчас 30 478,62 ₽. Текущий баланс всех счетов составляет 1 250 000 ₽."
`;

                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });

                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas автоматически предоставит ключ API во время выполнения
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();
                
                // Удаляем индикатор загрузки
                chatMessagesContainer.removeChild(loadingMessageDiv);

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const aiResponse = result.candidates[0].content.parts[0].text;
                    addMessage(aiResponse, 'ai');
                } else {
                    addMessage("Извините, не удалось получить ответ от AI-ассистента. Пожалуйста, попробуйте еще раз.", 'ai');
                }
            } catch (error) {
                console.error('Ошибка при вызове Gemini API:', error);
                // Удаляем индикатор загрузки в случае ошибки
                if(chatMessagesContainer.contains(loadingMessageDiv)) {
                    chatMessagesContainer.removeChild(loadingMessageDiv);
                }
                addMessage("Произошла ошибка при обработке вашего запроса. Пожалуйста, проверьте ваше интернет-соединение или попробуйте позже.", 'ai');
            }
        }

        // Инициализация страницы "Мой Бизнес" при загрузке документа
        document.addEventListener('DOMContentLoaded', () => {
            showPage('dashboard');
        });
    </script>
</body>
</html>
