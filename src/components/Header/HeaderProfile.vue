<template>
    <div class="dropdown dropdown-end">
      <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
        <div class="w-10 rounded-full">
          <img
            alt="Tailwind CSS Navbar component"
            src="/src/assets/icons/jpg/image-profile.jpg" />
        </div>
      </div>
      <ul
        tabindex="-1"
        class="menu menu-sm dropdown-content bg-base-300 dark:bg-base-300 rounded-box z-1 mt-3 w-52 p-2 shadow">
        <li>
          <a href="https://github.com/lucasmarques2020" target="_blank" class="justify-between dark:text-base-content text-neutral-900 hover:text-base-content">
            Github
          </a>
        </li>
        <li>
          <a href="https://br.linkedin.com/in/lucasmarquesdesignweb" target="_blank" class="dark:text-base-content text-neutral-900 hover:text-base-content">
            Linkedin
          </a>
        </li>
      </ul>
    </div>
</template>