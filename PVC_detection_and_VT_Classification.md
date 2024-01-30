# PVC-detection-and-classification-using-ECG-features
Characteristic matching algorithm detects PVC beats using three ECG feature sets (RR interval, T magnitude, and R magnitude) and adaptive decision logic based classifier classifies detected PVC beats into ventricular tachycardias
Algorithm 1 Pseudocode to Detect the PVC Beats Using Characteristic Matching Algorithm 
Finding ECG Beats Common to Any Two of the Three Feature Sets (FS1, FS2, FS3) 
Input Arrays Feature Set 1 = FS1 [i], Feature Set 2 = FS2 [j], Feature Set 3 = FS3 [k]
Input Array Size s1= maximum (i), s2= maximum (j), s3= maximum (k)
if((i<s1), (j<s2), (k<s3))
    if((i==j)|(j==k)|(k==i))
         if((i==j), (j!=k), (k!=i))
            Test Set [l]=FS1[i];
             l++;
             if(FS3[k]>FS1[i])
               i++;  j++;
            else if(FS3[k]<FS1[i])
              k++;
            end if
      end if
      else if((i!=j)|(j==k)|(k!=i))
           Test Set [l]=FS2[j];
           l++;
            if(FS1[i]>FS2[j])
               j++; k++;
            else if(FS1[i]<FS2[j])
               i++;
            end if
      end if
      else if((i!=j),(j!=k), (k==i))
           Test set [l]= FS3[k];
            l++;
            if(FS2[j]>FS3[k])
              i++; k++;
            else if(FS2[j]<FS3[k])
              j++;
            end if
      end if
  end if
  else if((i==j),(j==k), (k==i))
        Test Set [l]=FS1[i];
         l++; i++; j++; k++;
  end if 
  else if((i!=j),(j!=k), (k!=i))
       if((FS1[i]<FS2[j]), (FS1[i]<FS2[k]))
           i++;
       end if
       else if((FS1[j]<FS2[i]), (FS1[j]<FS2[k]))
          j++;
       end if
       else if((FS1[k]<FS2[i]), (FS1[k]<FS2[j]))
          k++;
       end if
  end if
end if
n: Total number of input ECG beat numbers
for(n=1…n)
      if(Test Set [n]==n)
            n++;
      else if(Test Set[n]!=n)
           Non_PVC Set[o]=n;
           n++; o++;
      end if
end for
Find the ECG beats in Test Set with the largest Rpeak magnitudes
if(p2<n)
    x1=Test Set [p1];
    x2=Non_PVC Set [p2];
    if(Rpeak magnitudes[x1]>Rpeak magnitudes[x2])
         p2++;
    end if
    else if(Rpeak magnitudes[x1]<Rpeak magnitudes[x2])
          p1++; p2=1;
    end if
else if(p2>n)
     PVC_detected Locations [r]=x1;
     r++; p1++; p2=1;
end if


Algorithm 2: Pseudocode to Classify Detected PVC Beats Using
Adaptive Decision-Based Classifier 
 Input Array: PVC_detected Locations [i] 
Classifying into Monomorphic or Polymorphic
if(j<i)
    x1=PVC_detected Locations[j];
    x2=PVC_detected Locations [j+1];
    if(Rpeak magnitude[x1][15]== Rpeak magnitude[x2][15])
       j++;
    else if (Rpeak magnitude[x1][15]!= Rpeak magnitude[x2][15])
       Polymorphic=1;
       Monomorphic=0;
  end if
end if
else if(j>=i)
    Polymorphic=0;
    Monomorphic=1;
end if
Classifying into NSVT and SVT
if(k<i)
    x3=PVC_detected Locations [k];
    x4=PVC_detected Locations [k+1];
    Difference[k]=x4-x3;
    k++;
end if
Initialize n=0
if(m<k)
     if(Difference[m]==1)
         m++;
         n++;
     else (Difference[m]!=1)
         NSVT=0;
        SVT=0;
         m++;
end if
else if(m>=k)
     if((n<=30),(HR>100))
          NSVT=1;
          SVT=0;
     else if((n>30),(HR>100))
          NSVT=0;
          SVT=1;
     end if
      else
           NSVT=0;
           SVT  =0;
end if
