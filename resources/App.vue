<template>
    <div id="app">
        <section class="section">
            <h4>
                Vue adoptation of Ettric's
                <a href="//codepen.io/ettrics/pen/QbPEeg">Codepen</a>
            </h4>
        </section>
        <kanban-board
            :stages="statuses"
            :blocks="blocksClone"
            :state-machine-config="stateMachineConfig"
            @update-block="updateBlock"
        >
            <div v-for="stage in statuses" :slot="stage" :key="stage">
                <h2>
                    {{ stage }}
                    <a>+</a>
                </h2>
            </div>
            <div v-for="item in blocks" :slot="item.id" :key="item.id">
                <div><strong>id:</strong> {{ item.id }}</div>
                <div>
                    {{ item.title }}
                </div>
            </div>
        </kanban-board>
    </div>
</template>

<script>
import faker from "faker";
import { debounce, cloneDeep } from "lodash";
import "vue-kanban/src/assets/kanban.scss";

export default {
    name: "app",
    data() {
        return {
            statuses: ["on-hold", "in-progress", "needs-review", "approved"],
            blocks: [],
            blocksClone: [],
            stateMachineConfig: {
                id: "kanban",
                initial: "on-hold",
                states: {
                    "on-hold": {
                        on: {
                            PICK_UP: "in-progress",
                        },
                    },
                    "in-progress": {
                        on: {
                            RELINQUISH_TASK: "on-hold",
                            PUSH_TO_QA: "needs-review",
                        },
                    },
                    "needs-review": {
                        on: {
                            REQUEST_CHANGE: "in-progress",
                            PASS_QA: "approved",
                        },
                    },
                    approved: {
                        type: "final",
                    },
                },
            },
        };
    },
    mounted() {
        for (let i = 0; i <= 10; i += 1) {
            this.blocks.push({
                id: i,
                status: this.statuses[Math.floor(Math.random() * 4)],
                title: faker.company.bs(),
            });

            this.blocksClone = cloneDeep(this.blocks);
        }
    },
    methods: {
        updateBlock: debounce(function (id, status, index) {
            console.log(`id`, id);
            console.log(`status`, status);
            console.log(`index`, index);
            this.blocks.find((b) => b.id === Number(id)).status = status;
        }, 500),
    },
};
</script>

<style lang="scss">
$on-hold: #fb7d44;
$in-progress: #2a92bf;
$needs-review: #f4ce46;
$approved: #00b961;
* {
    box-sizing: border-box;
}
body {
    background: #33363d;
    color: white;
    font-family: "Lato";
    font-weight: 300;
    line-height: 1.5;
    -webkit-font-smoothing: antialiased;
}
.drag-column {
    .drag-column-header > div {
        width: 100%;
        h2 > a {
            float: right;
        }
    }
    &-on-hold {
        .drag-column-header,
        .is-moved,
        .drag-options {
            background: $on-hold;
        }
    }
    &-in-progress {
        .drag-column-header,
        .is-moved,
        .drag-options {
            background: $in-progress;
        }
    }
    &-needs-review {
        .drag-column-header,
        .is-moved,
        .drag-options {
            background: $needs-review;
        }
    }
    &-approved {
        .drag-column-header,
        .is-moved,
        .drag-options {
            background: $approved;
        }
    }
}
.section {
    padding: 20px;
    text-align: center;
    a {
        color: white;
        text-decoration: none;
        font-weight: 300;
    }
    h4 {
        font-weight: 400;
        a {
            font-weight: 600;
        }
    }
}
</style>
