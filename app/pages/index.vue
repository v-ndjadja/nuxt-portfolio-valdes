<script setup>
</script>
<template>
    <div class="flex flex-col min-h-screen justify-between bg-slate-900">
      <div style="position: sticky ; top:0 ;z-index:100;color: var(--foregroundt);background-color: var(  --muted) ;" class="flex justify-between items-center px-10 py-2 b-border-gray-500">
            <div  class="text-white text-xs font-mono"> 
                <p style="font-size:24px" class=" flex items-start justify-center font-bold text-xl">
                    <strong>ndjadjavaldes.me <label>titre</label></strong>
                </p>
</div>
<nav > 
            <ul class=" text-white flex items-center gap-4">
                <li><a
                    href="#ACCUEIL"class="menu-link">ACCUEIL</a>

                </li>
                <li><a href="#A PROPOS"class="menu-link">A PROPOS</a>

                </li>
                <li><a href="#PARCOURS"class="menu-link">PARCOURS</a>

                </li>
                <li>
                    <a href="#COMPETENCES"class="menu-link">COMPETENCES</a>
                </li>
                <li><a href="#PROJETS"class="menu-link">PROJETS</a>

                </li>
                <li>
                    <a href="#CONTACTS"class="menu-link">CONTACT</a>
                </li>

            </ul>
        </nav>


        </div>

<div>
<HeaderNav
/>
</div>
<APPimage/>
<div class="container mx-auto py-10 px-4">
   <MainContent/>
</div>
<div>
   </div>
   <FooterNav/>
</div>
</template>
<script setup>

   </script>
<style scoped>
.menu-link{
   transition: color 0.3s ease;
   text-decoration: none;
}
.menu-link:hover{
   color: chocolate;

}
</style>
