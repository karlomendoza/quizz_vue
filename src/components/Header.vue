<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quizz App</b></b-nav-item>
            <b-nav-item disabled>Counter: {{totalCorrect}}/{{totalAnswers}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>

export default {
    props: [
        'totalCorrect',
        'totalAnswers'
    ]
}
</script>
