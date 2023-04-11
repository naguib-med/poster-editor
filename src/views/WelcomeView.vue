<template>
    <v-container class="welcome-view v-container--fluid">
        <h1>Bienvenue sur Poster Editor</h1>
        <p>Poster Editor est un outil de création de posters pour les événements de l'association.</p>
        <p>Commencez par créer un poster en cliquant sur le bouton ci-dessous.</p>
        <v-btn color="primary" size="x-large" to="/editor">Créer un poster</v-btn>
    </v-container>
</template>

<style scoped>
.welcome-view {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f1f1f1;
}


@media (min-width: 1024px) {
    .welcome-view {
        padding: 0 2rem;
    }
}

.welcome-view h1 {
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 1rem;
}

.welcome-view p {
    font-size: 1.25rem;
    font-weight: 400;
    margin-bottom: 2rem;
}

@media (min-width: 1024px) {
    .welcome-view h1 {
        font-size: 3rem;
    }

    .welcome-view p {
        font-size: 1.5rem;
    }

    .welcome-view button {
        font-size: 1.25rem;
    }
}


</style>
<script setup>
</script>