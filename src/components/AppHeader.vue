<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <router-link :to="{ name: 'contactbook' }" class="navbar-brand">
            Ứng dụng quản lý danh bạ
        </router-link>

        <div class="navbar-nav">
            <router-link :to="{ name: 'contactbook' }" class="nav-link">
                Danh bạ
                <i class="fas fa-address-book"></i>
            </router-link>
        </div>
    </nav>
</template>
