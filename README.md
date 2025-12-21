<button mat-icon-button
        class="close-button"
        [mat-dialog-close]="true">
  <mat-icon class="close-icon">close</mat-icon>
</button>

<!-- Error State -->
@if (!response?.data) {
  <div>
    <mat-icon color="warn"
              class="response-icon">error_outline</mat-icon>
    An error has occured!
  </div>
}

<!-- Content State -->
@if (response?.data?.online === false) {
  <!-- Offline State -->
  <div class="flex flex-col gap-[10px]">
    
    <!-- Status Row -->
    <div class="container flex flex-row items-center justify-center gap-[10px]">
      <div>Status</div>
      <mat-chip-list>
        <mat-chip class="mat-chip-style-error">Offline</mat-chip>
      </mat-chip-list>
    </div>

    <!-- Details Row -->
    <div class="container shadow flex flex-row flex-wrap items-center justify-start gap-[10px]">
       <div class="field-value w-full">
         {{ response?.data?.displayMessage?.displayMessageTitle }}
       </div>
       <div class="field sub-container shadow mt-3 flex-1">
         {{ response?.data?.displayMessage?.displayMessageBody }}
       </div>
    </div>
  
  </div>
} @else {
  <!-- Online State (Else Block) -->
  <div class="container flex flex-row items-center justify-center gap-[10px]">
    <div>Status</div>
    <mat-chip-list>
      <mat-chip class="mat-chip-style-success">Online</mat-chip>
    </mat-chip-list>
  </div>
}

<!-- Actions -->
<div mat-dialog-actions>
  <button mat-raised-button
          [mat-dialog-close]="true"
          cdkFocusInitial>Ok</button>
</div>

    // 24-hour format "09:30"
    const match24 = str.match(/^(\d{1,2}):(\d{2})$/);
    if (match24) {
      return parseInt(match24[1], 10) * 60 + parseInt(match24[2], 10);
    }
    return null;
  }
}
