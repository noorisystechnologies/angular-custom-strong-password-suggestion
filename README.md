<!-- Password Strength Meter  -->

**Step 1:** npm install

npm install @zxcvbn-ts/core @zxcvbn-ts/language-en angular-password-strength-meter --save

**Step 2:** Import Password Strength Meter Module into your app module (app.module.ts)

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { ReactiveFormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { PasswordStrengthComponent } from './components/password-strength/password-strength.component';

@NgModule({
  declarations: [
    AppComponent, 
    PasswordStrengthComponent
    ],
  imports: [
    BrowserModule, 
    ReactiveFormsModule]
    ,
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}


**Step 3:** use the password-strength-meter component in your app.component.html
  <password-strength-meter [password]="password"></password-strength-meter>

<div class="row">
  <div class="col-12 col-md-4 offset-md-4">
    <div class="card shadow-sm" *ngIf="!complete">
      <div class="card-body">
        <form [formGroup]="signupForm" (ngSubmit)="onSubmit()" novalidate autocomplete="off">
          <div class="form-group">
            <!-- email -->
            <label for="email" class="control-label font-weight-bold">Email</label>
            <input type="email" class="form-control" formControlName="email" placeholder="Email address"
              [ngClass]="{ 'is-valid': (submitted || f.email.dirty) && !f.email.errors, 'is-invalid': (submitted || f.email.dirty) && f.email.errors }">
            <div class="invalid-feedback" *ngIf="f.email.errors">
              <span *ngIf="f.email.errors?.required">Email address is required</span>
              <span *ngIf="f.email.errors?.email">Email address is not valid</span>
            </div>
          </div>    
          <div class="form-group">
            <label for="password" class="control-label font-weight-bold">Password</label>
            <input type="password" class="form-control" formControlName="password" placeholder="Choose a password" autocomplete="new-password"
              [ngClass]="{ 'is-valid': (submitted || f.password.dirty) && !f.password.errors, 'is-invalid': (submitted || f.password.dirty) && f.password.errors }">
              <!-- password -->
            <app-password-strength [passwordToCheck]="signupForm.value.password" 
            (passwordStrength)="onPasswordStrengthChanged($event)">
            </app-password-strength>    
            <!-- Password End   -->
            <div class="invalid-feedback" *ngIf="f.password.errors">
              <span *ngIf="f.password.errors?.required">Password is required</span>
              <span *ngIf="f.password.errors?.minlength">
                Your password must be 8-20 characters long , contain capital letters , small letters , special characters and numbers, and must not contain spaces or emoji.</span>
            </div>
          </div>    
          <button type="submit" class="btn btn-block btn-primary" [disabled]="signupForm.invalid || !strongPassword || working">
            {{ working ? 'Working on it...' : 'Create account' }}
          </button>
        </form>
      </div>
    </div>
    <div class="card text-white bg-success shadow-sm h-100" *ngIf="complete">
      <div class="card-header">Registration complete</div>
      <div class="card-body">
        <h5 class="card-title">You're all set 🐉</h5>
        <p class="card-text">Check your inbox to confirm your account.</p>
      </div>
    </div>
  </div>
</div>

**Step 4:** TypeScript Code For password-strength-meter

import { Component } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})

export class AppComponent {
  submitted = false;
  working = false;
  complete = false;
  strongPassword = false;

  signupForm = new FormGroup({
    email: new FormControl(null, [Validators.email, Validators.required]),
    password: new FormControl(null, [
      Validators.minLength(8),
      Validators.required,
    ]),
  });

  get f() {
    return this.signupForm.controls;
  }

  onPasswordStrengthChanged(event: boolean) {
    this.strongPassword = event;
  }

  onSubmit() {
    this.submitted = true;

    if (this.signupForm.invalid) {
      return;
    }

    this.working = true;
    setTimeout(() => {
      this.signupForm.reset();
      this.working = false;
      this.complete = true;
    }, 1000);
  }
}

 **Step 5:** Please Refer password-strength component files for password validation CSS & Design