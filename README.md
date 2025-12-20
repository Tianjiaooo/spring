npx -p @angular/cli@21 ng new MKMAppsManagerUI


    <nav class="bg-white/80 backdrop-blur-xl border-b border-gray-200/80 px-6 py-3 fixed left-0 right-0 top-0 z-30 sm:ml-64 transition-all duration-300 shadow-sm supports-[backdrop-filter]:bg-white/60">
      <div class="flex flex-wrap justify-between items-center h-full max-w-7xl mx-auto">
        
        <!-- Left: Breadcrumbs -->
        <div class="flex items-center">
           <div class="flex items-center gap-2 text-sm font-medium text-slate-500">
             <a routerLink="/dashboard" class="hover:text-blue-600 hover:bg-slate-100 px-2 py-1 rounded-md transition-all cursor-pointer flex items-center gap-2 group">
               <svg class="w-4 h-4 text-slate-400 group-hover:text-blue-500 transition-colors" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                 <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
               </svg>
               <span class="hidden sm:inline">Dashboard</span>
             </a>
             
             <svg class="w-4 h-4 text-slate-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
               <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
             </svg>
             
             <!-- Page Title -->
             <span class="text-slate-800 font-bold tracking-tight px-1">{{ pageTitle() }}</span>
           </div>
        </div>

        <!-- Right: User Actions -->
        <div class="flex items-center gap-4 md:gap-6">
          
          @if (loginService.userInfo()) {
            <div class="flex items-center gap-3">
              <!-- Notification Bell -->
              <button class="p-2 text-slate-400 hover:text-slate-600 hover:bg-slate-100 rounded-full transition-colors relative">
                <span class="absolute top-2 right-2 w-2 h-2 bg-red-500 rounded-full border-2 border-white"></span>
                <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                </svg>
              </button>

              <div class="h-6 w-px bg-slate-200 mx-1"></div>

              <!-- Profile Dropdown Trigger -->
              <button class="flex items-center gap-3 focus:outline-none group p-1 pr-3 rounded-full hover:bg-slate-50 transition-all border border-transparent hover:border-slate-200"
                      (click)="isOpen.set(!isOpen())"
                      cdkOverlayOrigin
                      #trigger="cdkOverlayOrigin">
                
                <div class="w-8 h-8 rounded-full bg-gradient-to-br from-blue-600 to-indigo-700 flex items-center justify-center text-white font-bold text-xs shadow-md shadow-blue-500/20 ring-2 ring-white">
                  {{ getUserInitials() }}
                </div>

                <div class="text-sm font-medium text-slate-700 group-hover:text-slate-900 transition-colors hidden md:block">
                  {{ userName() }}
                </div>
                
                <svg class="w-4 h-4 text-slate-400 group-hover:text-slate-600 transition-colors" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                </svg>
              </button>
            </div>

            <!-- Dropdown Template -->
            <ng-template
              cdkConnectedOverlay
              [cdkConnectedOverlayOrigin]="trigger"
              [cdkConnectedOverlayOpen]="isOpen()"
              [cdkConnectedOverlayHasBackdrop]="true"
              [cdkConnectedOverlayBackdropClass]="'cdk-overlay-transparent-backdrop'"
              (backdropClick)="isOpen.set(false)"
              [cdkConnectedOverlayOffsetY]="8">
              
              <div class="w-64 bg-white rounded-xl shadow-[0_10px_40px_-10px_rgba(0,0,0,0.15)] border border-gray-100 py-2 animation-fade-in ring-1 ring-black/5">
                <!-- User Info -->
                <div class="px-4 py-3 border-b border-gray-50 bg-slate-50/50">
                  <p class="text-xs text-slate-500 font-semibold uppercase tracking-wider mb-1">Signed in as</p>
                  <p class="text-sm text-slate-900 font-semibold truncate">{{ loginService.userInfo()?.email }}</p>
                  <p class="text-xs text-blue-600 font-medium mt-0.5">Administrator</p>
                </div>
                
                <div class="py-1">
                  <a href="#" class="flex items-center gap-3 px-4 py-2.5 text-sm text-slate-600 hover:bg-slate-50 hover:text-slate-900 transition-colors group">
                    <div class="p-1.5 rounded-md bg-slate-100 text-slate-500 group-hover:bg-white group-hover:text-blue-600 group-hover:shadow-sm transition-all">
                       <svg class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" /><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
                    </div>
                    <div>
                      <span class="block font-medium">Account Settings</span>
                      <span class="block text-xs text-slate-400 font-normal">Manage your preferences</span>
                    </div>
                  </a>
                </div>

                <div class="border-t border-gray-100 my-1"></div>
                
                <div class="py-1 px-1">
                  <button (click)="logout()" class="flex w-full items-center gap-2 px-3 py-2 text-sm text-red-600 hover:bg-red-50 rounded-lg transition-colors font-medium">
                    <svg class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" /></svg>
                    Sign out
                  </button>
                </div>
              </div>
            </ng-template>
          }
        </div>
      </div>
    </nav>
