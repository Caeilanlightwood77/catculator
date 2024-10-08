<script>
export default {
    name: "CatApp",
    data() {
        return {
            display: "",
            output: "",
            buttons: [
                { label: "(", value: "(", color: "blue-grey lighten-4" },
                { label: ")", value: ")", color: "blue-grey lighten-4" },
                { label: "%", value: "%", color: "blue-grey lighten-4" },
                { label: "AC", value: "clear", color: "red lighten-1" },
                { label: "7", value: "7", color: "grey lighten-3" },
                { label: "8", value: "8", color: "grey lighten-3" },
                { label: "9", value: "9", color: "grey lighten-3" },
                { label: "÷", value: "/", color: "blue-grey lighten-4" },
                { label: "4", value: "4", color: "grey lighten-3" },
                { label: "5", value: "5", color: "grey lighten-3" },
                { label: "6", value: "6", color: "grey lighten-3" },
                { label: "×", value: "*", color: "blue-grey lighten-4" },
                { label: "1", value: "1", color: "grey lighten-3" },
                { label: "2", value: "2", color: "grey lighten-3" },
                { label: "3", value: "3", color: "grey lighten-3" },
                { label: "-", value: "-", color: "blue-grey lighten-4" },
                { label: "DEL", value: "backspace", color: "red lighten-1" },
                { label: "0", value: "0", color: "grey lighten-3" },
                { label: ".", value: ".", color: "blue-grey lighten-4" },
                { label: "+", value: "+", color: "blue-grey lighten-4" },
                { label: "=", value: "=", color: "green accent-4" },
                { label: "^", value: "**", color: "blue-grey lighten-4" },
            ],
        };
    },
    methods: {
        handleButtonClick(button) {
            if (button.value === "clear") {
                this.clearAll();
            } else if (button.value === "backspace") {
                this.backspace();
            } else if (button.value === "=") {
                this.calculate();
            } else {
                this.appendToDisplay(button.value);
            }
        },
        appendToDisplay(value) {
            this.display += value;
        },
        clearAll() {
            this.display = "";
            this.output = "";
        },
        backspace() {
            this.display = this.display.slice(0, -1);
        },
        calculate() {
            try {
                let userInput = this.display.replace(/([\d.]+)(?=\()/g, "$1*");
                if (!userInput.trim()) {
                    this.output = "No expression";
                    setTimeout(() => (this.output = ""), 2000);
                    return;
                }
                const result = eval(userInput);
                if (!isNaN(result) && isFinite(result)) {
                    this.output = result;
                    setTimeout(() => (this.display = ""), 250);
                } else {
                    throw new Error("Error");
                }
            } catch (error) {
                this.output = "Invalid expression";
                setTimeout(() => (this.output = ""), 2000);
            }
        },
        handleKeyPress(event) {
            const key = event.key;
            if ("0123456789".includes(key)) {
                this.appendToDisplay(key);
            } else if (key === "Enter") {
                event.preventDefault();
                this.calculate();
            } else if (key === "Escape") {
                this.clearAll();
            } else if (key === "Backspace") {
                this.backspace();
            } else if ("()+-*/.%".includes(key)) {
                this.appendToDisplay(key === "/" ? "/" : key);
            } else if (key === "^") {
                this.appendToDisplay("**");
            }
        },
    },
    mounted() {
        this.$el.focus();
    },
};
</script>

<template>
    <v-container fluid class="calculator-container" @keydown="handleKeyPress" tabindex="0">
        <v-row class="justify-center">
            <v-col cols="12" sm="6" md="4">
                <v-card class="pa-4 calculator-card">
                    <!-- Display Screens -->
                    <v-text-field v-model="output" readonly class="output-screen mb-2" hide-details></v-text-field>
                    <v-text-field v-model="display" readonly class="calculator-screen mb-4" hide-details></v-text-field>

                    <!-- Calculator Keys -->
                    <v-row dense>
                        <v-col cols="3" v-for="button in buttons" :key="button.value">
                            <v-btn :color="button.color" class="calculator-button" @click="handleButtonClick(button)"
                                depressed large>
                                {{ button.label }}
                            </v-btn>
                        </v-col>
                    </v-row>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<style scoped>
.calculator-container {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background-image: url('/src/assets/Images/cat\ copy1.jpg');
    /* Keeping the cat background */
    background-size: cover;
    background-position: center;
}

.calculator-card {
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    background-color: rgba(255, 255, 255, 0.9);
    /* Slightly translucent card */
}

.output-screen,
.calculator-screen {
    background-color: #2c3e50;
    color: white;
    font-size: 1.8rem;
    text-align: right;
    border-radius: 8px;
}

.calculator-button {
    font-size: 1.5rem;
    font-weight: bold;
    transition: all 0.2s ease;
}

.calculator-button:hover {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.calculator-button:active {
    transform: scale(0.98);
}
</style>