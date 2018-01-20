Перед вами описание будущего эмулятора игры в покер.
Приложение будет иметь три режима: до начала игры, во время игры и после окончания игры (турнира); один функциональный экран (во время игры) и два всплывающих окна (до начала игры и после окончания). До начала игры - окно с кнопкой «Начать новую игру», после окончания игры - окно с результатом прошедшей игры и кнопка «Новая игра».

Основной экран схематично изображен на рисунке: <img src="mockup_poker.jpg">

Кружки, внутри которых значения переменной name игроков, рядом с каждым из которых количество фишек stack и размер текущей ставки bet. Игрок в верхней части экрана - противник, мы - внизу. Справа от нашего игрока кнопки (в зависимости от хода раздачи, то есть возможных дальнейших действий) check и bet / fold, call, raise; в середине игрового поля карты (dealercontainer).

Логика работы кода: 

Вся игра строится на взаимодействии с сервером - оттуда приходят события и значения переменных. 

Всего событий 8:
- start начало нового уровня. Приходит массив объектов [{playerId:name},{playerId:name}], отображаются игроки;   
- gamestart приходит объект stacks = {playerId: [stack]; playerId: [stack]};               
- level расклада карт на игровом поле. В данных приходит переменная name, значение которой preFlop, flop, turn или river,  в зависимости от хода игры. В случае одного из трех последних значений также приходит размер банка pot, обновляется переменная pot на игровом поле. Также в данных приходит массив открываемых карт cards,  случае preFlop приходит массив карт нашего игрока. Также приходит обновленный stacks;     
- turnrequest запрос хода «нашего» игрока, становятся активными кнопки в плевой части игрового поля;   
- turn очередной ход одного из игроков. Также приходит обновленный stacks, обновляются bet и stack;   
- showdown вскрываем карты. Приходит объект с картами игроков {playerId: [cards], playerId: [cards]};    
- gameend конец раздачи, приходит объект {winnerId, pot};                     
- end конец турнира, приходит объект {winnerId}, что и выводится на всплывающем окне.

Взаимодействие с сервером будет реализовано с помощью websockets (выше описаны события, которые будут приходить от сервера), с помощью метода fetch мы будем передавать информацию о пользовательских событиях на сервер(нажатия на кнопки).
