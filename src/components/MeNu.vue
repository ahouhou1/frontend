<template>
    <nav>
        <ul>
            <li><a href="#">Ahmad's Shop</a></li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'MeNu'
};
</script>

<style scoped>
nav {
    background-color: #4CAF50;
    color: white;
    padding: 1.5em;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

li {
    display: inline-block; 
    margin-right: 1.5em;
}

a {
    color: white;
    text-decoration: none;
    font-weight: bold; 
    transition: color 0.3s ease;
}

a:hover {
    color: #FFD700;
}
</style>
