<template>  
<h1>Admin Panel</h1>
                <AdminNav />
</template>


<style scoped>

h1{
        text-align: center;
}
</style>

<script>
import AdminNav from '@/components/AdminNav.vue';

export default{
        name: 'Adminhomepage',
        components:{
                AdminNav
        }
}
</script>