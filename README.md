npx -p @angular/cli@21 ng new MKMAppsManagerUI
    <nav class="bg-white/90 backdrop-blur-md border-b border-gray-200/60 px-6 py-3 fixed left-0 right-0 top-0 z-30 sm:ml-64">
      <div class="flex flex-wrap justify-between items-center h-full">
        
        <!-- Left: Dynamic Breadcrumbs / Title -->
        <div class="flex items-center">
           <div class="flex items-center gap-2 text-sm text-slate-500 font-medium">
             <a routerLink="/dashboard" class="hover:text-blue-600 hover:bg-blue-50 px-2 py-1 rounded-md transition-all cursor-pointer flex items-center gap-1 group">
               <svg class="w-4 h-4 text-slate-400 group-hover:text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                 <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
               </svg>
               Dashboard
             </a>
             <svg class="w-4 h-4 text-slate-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
               <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
             </svg>
             <span class="text-slate-900 font-semibold px-1">{{ pageTitle() }}</span>
           </div>
        </div>

        <!-- Right: User Actions -->
        <div class="flex items-center gap-6">
          
          @if (loginService.userInfo()) {
            <!-- User Profile Dropdown Area using CDK Overlay -->
            <button class="flex items-center gap-3 focus:outline-none group"
                    (click)="isOpen.set(!isOpen())"
                    cdkOverlayOrigin
                    #trigger="cdkOverlayOrigin">
              <div class="text-right hidden md:block">
                <div class="text-sm font-semibold text-slate-800 group-hover:text-blue-600 transition-colors">
                  {{ userName() }}
                </div>
                <div class="text-xs text-slate-500 font-medium">Administrator</div>
              </div>
              
              <div class="w-10 h-10 rounded-full bg-gradient-to-br from-blue-500 to-indigo-600 flex items-center justify-center text-white font-bold text-sm shadow-md ring-2 ring-white group-hover:ring-blue-100 transition-all">
                {{ getUserInitials() }}
              </div>
            </button>

            <!-- Dropdown Template -->
            <ng-template
              cdkConnectedOverlay
              [cdkConnectedOverlayOrigin]="trigger"
              [cdkConnectedOverlayOpen]="isOpen()"
              [cdkConnectedOverlayHasBackdrop]="true"
              [cdkConnectedOverlayBackdropClass]="'cdk-overlay-transparent-backdrop'"
              (backdropClick)="isOpen.set(false)"
              [cdkConnectedOverlayOffsetY]="10">
              
              <div class="w-48 bg-white rounded-xl shadow-lg border border-gray-100 py-1 animation-fade-in">
                <div class="px-4 py-3 border-b border-gray-50">
                  <p class="text-sm text-gray-900 font-medium truncate">{{ loginService.userInfo()?.email }}</p>
                </div>
                <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 transition-colors">Settings</a>
                <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50 transition-colors">Support</a>
                <div class="border-t border-gray-50 my-1"></div>
                <button (click)="logout()" class="block w-full text-left px-4 py-2 text-sm text-red-600 hover:bg-red-50 transition-colors font-medium">
                  Sign out
                </button>
              </div>
            </ng-template>
          }
        </div>
      </div>
    </nav>
