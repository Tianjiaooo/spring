import { Injectable } from '@angular/core';
import { AbstractControl, FormGroup, ValidationErrors, ValidatorFn } from '@angular/forms';
import { FormRule, FormStatusEnum } from '../models/forms.model';
import { FormsService } from './forms.service';

@Injectable({
  providedIn: 'root'
})
export class ValidatorsService {

  constructor(private formsService: FormsService) { }

  /**
   * Validate against parent form
   * - Ensure only one rule is active at any time
   * @returns
   */
  public allRulesValidator(): ValidatorFn {
    return (form: AbstractControl): ValidationErrors | null => {
      // console.log('allRulesValidator');
      let myForm = form as FormGroup;
      let activeCount = 0;

      if (myForm.get('formRules')) {
        let ruleDetails: FormRule[] = myForm.get('formRules')?.value;
        if (Array.isArray(ruleDetails)) {
          ruleDetails.forEach((element) => {
            if (element.active) {
              activeCount++;
            }
          });
        }
      }

      // console.log('active count {}:', activeCount);
      if (activeCount > 1) {
        return { onlyOneActiveRuleAllowed: 'Only one rule can be active' };
      }

      return null;
    };
  }

  /**
   * Validate rules for dates/times etc
   * @returns
   */
  public formRuleValidator(): ValidatorFn {
    return (form: AbstractControl): ValidationErrors | null => {
      const myControls = ['startDate', 'endDate', 'startTime', 'endTime'];
      const formGroup = form as FormGroup;
      
      for (let control of myControls) {
        formGroup.get(control)?.setErrors(null);
      }

      const status = formGroup.get('status')?.value;
      
      if (status === FormStatusEnum.offlineBetweenDatesTimes) {
        // Change 1: Receive as strings (Original values)
        const startDateStr = formGroup.get('startDate')?.value;
        const endDateStr = formGroup.get('endDate')?.value;
        const startTimeStr = formGroup.get('startTime')?.value;
        const endTimeStr = formGroup.get('endTime')?.value;

        // Change 2: Check for existence using strings (Fixes "Object is truthy" bug)
        if (!startDateStr) {
          formGroup.get('startDate')?.setErrors({ startDateRequired: 'Please enter a start date' });
          return { startDateRequired: 'Please enter a start date' };
        }
        if (!endDateStr) {
          formGroup.get('endDate')?.setErrors({ endDateRequired: 'Please enter an end date' });
          return { endDateRequired: 'Please enter an end date' };
        }
        if (!startTimeStr) {
          formGroup.get('startTime')?.setErrors({ startTimeRequired: 'Please enter a start time' });
          return { startTimeRequired: 'Please enter a start time' };
        }
        if (!endTimeStr) {
          formGroup.get('endTime')?.setErrors({ endTimeRequired: 'Please enter an end time' });
          return { endTimeRequired: 'Please enter an end time' };
        }

        // Change 3: Convert to Date objects ONLY when we need to compare them
        const startDate = new Date(startDateStr);
        const endDate = new Date(endDateStr);

        // Change 4: Compare dates strictly by Year/Month/Day (ignoring time)
        // Original: if (moment(startDate).isSame(endDate))
        if (this.isSameDate(startDate, endDate)) {
            
            // Change 5: Parse and compare times (minutes from midnight)
            const startMinutes = this.parseTimeMinutes(startTimeStr);
            const endMinutes = this.parseTimeMinutes(endTimeStr);

            // Original: startTimeMoment.isSameOrAfter(endTimeMoment)
            if (startMinutes !== null && endMinutes !== null) {
                if (startMinutes >= endMinutes) {
                    const error = { startTimeAfterEndTime: 'Start time cannot be the same or after end time' };
                    formGroup.get('startTime')?.setErrors(error);
                    return error;
                }
            }
        }
      }

      return null;
    };
  }

  // --- Helper Methods ---

  private isSameDate(d1: Date, d2: Date): boolean {
    if (isNaN(d1.getTime()) || isNaN(d2.getTime())) return false;
    return d1.getFullYear() === d2.getFullYear() &&
           d1.getMonth() === d2.getMonth() &&
           d1.getDate() === d2.getDate();
  }

  private parseTimeMinutes(timeStr: string): number | null {
    if (!timeStr) return null;
    const str = timeStr.toLowerCase().trim();

    // 12-hour format "9:30am"
    const match12 = str.match(/^(\d{1,2}):(\d{2})(am|pm)$/);
    if (match12) {
      let hours = parseInt(match12[1], 10);
      const minutes = parseInt(match12[2], 10);
      const meridian = match12[3];
      if (hours === 12) hours = (meridian === 'am' ? 0 : 12);
      else if (meridian === 'pm') hours += 12;
      return hours * 60 + minutes;
    }

    // 24-hour format "09:30"
    const match24 = str.match(/^(\d{1,2}):(\d{2})$/);
    if (match24) {
      return parseInt(match24[1], 10) * 60 + parseInt(match24[2], 10);
    }
    return null;
  }
}
