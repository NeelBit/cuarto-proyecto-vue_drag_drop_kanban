<script setup>

    import { reactive } from 'vue';

    import InputNew from './InputNew.vue';

    /* Variables reactivas: 
        ref para valores sencillos (números, string, bool...) 
        reactive para mas complejos (objetos o arreglos) */
    let boards = reactive([ // lista de objetos con sus propiedades
        {
            id: crypto.randomUUID(),
            name: "TO-DO",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Feature de archivos"
                },
                {
                    id: crypto.randomUUID,
                    title: "Resolver bug"
                }, */
            ]
        },
        {
            id: crypto.randomUUID(),
            name: "DOING",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Reporte 23"
                },
                {
                    id: crypto.randomUUID,
                    title: "Revisión disposición display"
                }, */
            ]
        },
        {
            id: crypto.randomUUID(),
            name: "DONE",
            items: [    // elementos que movemos de un tablero a otro
                /* {
                    id: crypto.randomUUID,
                    title: "Cambio en files"
                },
                {
                    id: crypto.randomUUID,
                    title: "Resolución bug arrow function pie de página"
                }, */
            ]
        },
    ]);

    
    function handleNewItem(text, board) {
        //console.log(text.value, board.id, board.name);
        board.items.push({
            id: crypto.randomUUID(),
            title: text.value,
        });
    }

    function handleNewBoard() {
        const name = prompt("Nombre del tablero: ");
        if (!!name) {
            boards.push({
                id: crypto.randomUUID(),
                name: name,
                items: []
            });
        }
    }

    // drag and drop:

    function startDrag(event, board, item) {
        
        event.dataTransfer.setData('text/plain', JSON.stringify({boardId: board.id, itemId: item.id}));

    }

    function onDrop(event, destino) {
        
        const { boardId, itemId} = JSON.parse(event.dataTransfer.getData('text/plain'));

        //console.log(`Soltado con los valores: boardId = ${boardId} e itemId = ${itemId}`);

        const originBoard = boards.find((board) => board.id === boardId);
        const originItem = originBoard.items.find((item) => item.id === itemId);


        // error con elementos sin id
        /* console.log(originBoard.name);
        console.log(originItem.title); */

        destino.items.push({ ...originItem });

        originBoard.items = originBoard.items.filter((item) => item !== originItem);

    }


</script>

<template>
    <nav class="nav">
        <ul>
            <li class="nav__item"><a href="#" @click.prevent="handleNewBoard">Crear board</a></li>
        </ul>
    </nav>

    <div class="boards-container">

        <div class="boards">
            <div class="board" v-for="b in boards" :key="b.id" @drop="onDrop($event, b)" @dragover.prevent @dragenter.prevent>
            
                <div class="board__name centrar-texto">
                    <h2>{{b.name}}</h2> 
                </div>

                <!-- emit -->
                <InputNew @on-new-item="(text) => handleNewItem(text, b)" />

                <div class="board__items"> 
                    <div class="item" v-for="i in b.items" :key="i.id" draggable="true" @dragstart="startDrag($event, b, i)">
                        {{i.title}}
                    </div>
                </div>
            
            </div>
        </div>

    </div>

</template>

<style scoped>

    .nav {
        background-color: var(--colorNegroRelax);
        
    }

    .nav ul {
        padding: .5rem;
        list-style: none;
    }

    .nav__item {
        
        text-transform: uppercase;
        padding: .5rem;
    }

    .nav__item a {
        color: var(--colorBlancoRelax);
        padding: .5rem;
        transition: background-color var(--d) ease-in-out;
    }

    .nav__item a:hover {
        background-color: var(--colorAzul1);
    }

    .boards-container {
        margin-bottom: var(--separacion);
    }

    .boards {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: var(--separacion);
        flex-wrap: wrap;
    }

    .board {
        width: 300px;
        min-height: 400px;
        padding: var(--separacion);
        /* border: red solid 1px; */
        background-color: var(--colorAzul2);
        border-radius: var(--redondeo);
        box-shadow: rgba(17, 17, 26, 0.1) 0px 4px 16px, rgba(17, 17, 26, 0.1) 0px 8px 24px, rgba(17, 17, 26, 0.1) 0px 16px 56px;
    }

    .item{
        background-color: var(--colorBlancoRelax);
        padding: 1rem;
        margin: 1rem 0;

        font-weight: bold;
        
    }

</style>