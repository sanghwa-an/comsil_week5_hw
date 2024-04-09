#include <iostream>
#include <sstream>
#include <chrono>
#include <thread>
#include <stdio.h>
#include <stdlib.h>

#define PNG_SETJMP_NOT_SUPPORTED
#include <png.h>

#define WIDTH 256
#define HEIGHT 256
#define COLOR_DEPTH 8

struct Pixel {
   png_byte r, g, b, a;
};

using namespace std;





int flag(string color, int life) {
   srand(time(NULL));

   /* open PNG file for writing */
   FILE *f = fopen("out.png", "wb");
   if (!f) {
      fprintf(stderr, "could not open out.png\n");
      return 1;
   }

   /* initialize png data structures */
   png_structp png_ptr;
   png_infop info_ptr;

   png_ptr = png_create_write_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
   if (!png_ptr) {
      fprintf(stderr, "could not initialize png struct\n");
      return 1;
   }

   info_ptr = png_create_info_struct(png_ptr);
   if (!info_ptr) {
      png_destroy_write_struct(&png_ptr, (png_infopp)NULL);
      fclose(f);
      return 1;
   }

   /* begin writing PNG File */
   png_init_io(png_ptr, f);
   png_set_IHDR(png_ptr, info_ptr, WIDTH, HEIGHT, COLOR_DEPTH,
                PNG_COLOR_TYPE_RGB_ALPHA, PNG_INTERLACE_NONE,
                PNG_COMPRESSION_TYPE_DEFAULT, PNG_FILTER_TYPE_DEFAULT);
   png_write_info(png_ptr, info_ptr);

   /* allocate image data */
   struct Pixel *row_pointers[HEIGHT];
   for (int row = 0; row < HEIGHT; row++) {
      row_pointers[row] = (Pixel *)calloc(WIDTH*2, sizeof(struct Pixel));
   }

   /* draw a bunch of vertical lines */
    for (int col =0; col < 256; col++) {
      
      for (int row = 0; row < 256; row++) {
         
            if(row<160){
                row_pointers[row][col].r =135; // red
                row_pointers[row][col].g = 206; // green
                row_pointers[row][col].b = 255; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else if(row<180){
                row_pointers[row][col].r =97; // red
                row_pointers[row][col].g = 54; // green
                row_pointers[row][col].b = 17; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else{
                row_pointers[row][col].r =139; // red
                row_pointers[row][col].g= 69; // green
                row_pointers[row][col].b = 19; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
        } 
    }
    for (int col = 30;col<50;col++){
        for(int row = 120 ; row<159;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 210; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 27;col<54;col++){
        for(int row = 100;row<119;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 255; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 90; col < 130;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =160; // red
            row_pointers[row][col].g = 82; // green
            row_pointers[row][col].b = 45; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 130;col<140;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =255; // red
            row_pointers[row][col].g = 191; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][200+col].r =255; // red
               row_pointers[90+row][200+col].g = 255; // green
               row_pointers[90+row][200+col].b = 255; // blue
               row_pointers[90+row][200+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][225+col].r =255; // red
               row_pointers[90+row][225+col].g = 255; // green
               row_pointers[90+row][225+col].b = 255; // blue
               row_pointers[90+row][225+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = 0;col<=30;col++){
        for(int row = 0; row <= 30; row++){
            if(row*row+col*col<=900){
               row_pointers[row][col].r =255; // red
               row_pointers[row][col].g = 0; // green
               row_pointers[row][col].b = 0; // blue
               row_pointers[row][col].a = 255; // alpha (opacity)
            }
        }
    }

   //깃발
   if(color=="2"){
      for (int col = 70; col < 120; col++) {
      
      for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 0; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 255; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
      }
      for(int col = 120;col<130;col++){
        for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 0; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 195; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
    }
      for(int col = 130; col<200;col++){
        for(int row = 50;row<140;row++){
            row_pointers[row][col].r = 0; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 255; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
      }
      
      for (int col = 65; col < 70; col++) {
      
      for (int row = 20; row < 256; row++) {
         
            row_pointers[row][col].r = 212; // red
            row_pointers[row][col].g = 212; // green
            row_pointers[row][col].b = 212; // blue
            row_pointers[row][col].a = 212; // alpha (opacity)
         }
      }
      for(int col =64;col <71;col++){
        row_pointers[19][col].r = 0; // red
        row_pointers[19][col].g = 0; // green
        row_pointers[19][col].b = 0; // blue
        row_pointers[19][col].a = 212; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][71].r = 0; // red
        row_pointers[row][71].g = 0; // green
        row_pointers[row][71].b = 0; // blue
        row_pointers[row][71].a = 0; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][64].r = 0; // red
        row_pointers[row][64].g = 0; // green
        row_pointers[row][64].b = 0; // blue
        row_pointers[row][64].a = 0; // alpha (opacity)
      }
   
   }
   if(color=="1"){
     for (int col = 70; col < 120; col++) {
      
      for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 255; // red
            row_pointers[row][col].g = 255; // green
            row_pointers[row][col].b = 255; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
      }
    for(int col = 120;col<130;col++){
        for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 215; // red
            row_pointers[row][col].g = 215; // green
            row_pointers[row][col].b = 215; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
    }
      for(int col = 130; col<200;col++){
        for(int row = 50;row<140;row++){
            row_pointers[row][col].r = 255; // red
            row_pointers[row][col].g = 255; // green
            row_pointers[row][col].b = 255; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
      }
       for(int col =64;col <71;col++){
        row_pointers[19][col].r = 0; // red
        row_pointers[19][col].g = 0; // green
        row_pointers[19][col].b = 0; // blue
        row_pointers[19][col].a = 212; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][71].r = 0; // red
        row_pointers[row][71].g = 0; // green
        row_pointers[row][71].b = 0; // blue
        row_pointers[row][71].a = 0; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][64].r = 0; // red
        row_pointers[row][64].g = 0; // green
        row_pointers[row][64].b = 0; // blue
        row_pointers[row][64].a = 0; // alpha (opacity)
      }

      for (int col = 65; col < 70; col++) {
      
      for (int row = 20; row < 256; row++) {
         
            row_pointers[row][col].r = 212; // red
            row_pointers[row][col].g = 212; // green
            row_pointers[row][col].b = 212; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         }
      }
   
   
   }
if(color=="3"){
      for (int col = 70; col < 120; col++) {
      
      for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 255; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
      }
    for(int col = 120;col<130;col++){
        for (int row = 40; row < 120; row++) {
         
            row_pointers[row][col].r = 215; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         } 
    }
      for(int col = 130; col<200;col++){
        for(int row = 50;row<140;row++){
            row_pointers[row][col].r = 255; // red
            row_pointers[row][col].g = 0; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
      }

      
 for(int col =64;col <71;col++){
        row_pointers[19][col].r = 0; // red
        row_pointers[19][col].g = 0; // green
        row_pointers[19][col].b = 0; // blue
        row_pointers[19][col].a = 212; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][71].r = 0; // red
        row_pointers[row][71].g = 0; // green
        row_pointers[row][71].b = 0; // blue
        row_pointers[row][71].a = 0; // alpha (opacity)
      }
      for(int row = 20;row<256;row++){
        row_pointers[row][64].r = 0; // red
        row_pointers[row][64].g = 0; // green
        row_pointers[row][64].b = 0; // blue
        row_pointers[row][64].a = 0; // alpha (opacity)
      }

      for (int col = 65; col < 70; col++) {
      
      for (int row = 20; row < 256; row++) {
         
            row_pointers[row][col].r = 212; // red
            row_pointers[row][col].g = 212; // green
            row_pointers[row][col].b = 212; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
         }
      }
   
   }
   if(color=="blank"){
      for (int col =0; col < 256; col++) {
      
      for (int row = 0; row < 256; row++) {
         
            if(row<160){
                row_pointers[row][col].r =135; // red
                row_pointers[row][col].g = 206; // green
                row_pointers[row][col].b = 255; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else if(row<180){
                row_pointers[row][col].r =97; // red
                row_pointers[row][col].g = 54; // green
                row_pointers[row][col].b = 17; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else{
                row_pointers[row][col].r =139; // red
                row_pointers[row][col].g = 69; // green
                row_pointers[row][col].b = 19; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
        } 
    }
    for (int col = 30;col<50;col++){
        for(int row = 120 ; row<159;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 210; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 27;col<54;col++){
        for(int row = 100;row<119;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 255; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 90; col < 130;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =160; // red
            row_pointers[row][col].g = 82; // green
            row_pointers[row][col].b = 45; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 130;col<140;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =255; // red
            row_pointers[row][col].g = 191; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][200+col].r =255; // red
               row_pointers[90+row][200+col].g = 255; // green
               row_pointers[90+row][200+col].b = 255; // blue
               row_pointers[90+row][200+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][225+col].r =255; // red
               row_pointers[90+row][225+col].g = 255; // green
               row_pointers[90+row][225+col].b = 255; // blue
               row_pointers[90+row][225+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = 0;col<=30;col++){
        for(int row = 0; row <= 30; row++){
            if(row*row+col*col<=900){
               row_pointers[row][col].r =255; // red
               row_pointers[row][col].g = 0; // green
               row_pointers[row][col].b = 0; // blue
               row_pointers[row][col].a = 255; // alpha (opacity)
            }
        }
    }

}

   
   
        for(int i=0;i<life;i++){
         int row,col;
         row=7;
         for(col=242-i*20;col<245-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=248-i*20;col<251-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         row=8;
         for(col=241-i*20;col<246-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=247-i*20;col<252-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         for(row=9;row<12;row++){
         for(col=240-i*20;col<253-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         }

         
         for(int j=0;j<6;j++){
         for(col=241-i*20+j;col<252-i*20-j;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         
         
         
         }
         row++;
         }
         
         

           
    
   }
        for(int i=0;i<3;i++){
         int col=253-i*20;
            for(int row=9;row<12;row++){
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
         }
         
         int row=12;
         for(col=252-i*20;col>246-i*20;col--){
            
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
            row++;
         }
         
         col=246-i*20;
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         
         row--;
         for(col=245-i*20;col>239-i*20;col--){
            
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
            row--;
         }

         col=239-i*20;
            for(int row=9;row<12;row++){
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
         }

         row=8;
         for(col=240-i*20;col<253-i*20;col+=6){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         
         row=7;
         for(col=241-i*20;col<246-i*20;col+=4){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=247-i*20;col<252-i*20;col+=4){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         
         row=6;
         for(col=242-i*20;col<245-i*20;col++){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=248-i*20;col<251-i*20;col++){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         row=9;
         col=242-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;

         row=10;
         col=241-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;
         
         row=11;
         col=242-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;
}
   
   
   /* write image data to disk */
   png_write_image(png_ptr, (png_byte **)row_pointers);

   /* finish writing PNG file */
   png_write_end(png_ptr, NULL);

   /* clean up PNG-related data structures */
   png_destroy_write_struct(&png_ptr, &info_ptr);

   /* close the file */
   fclose(f);
   f = NULL;

   return 0;
}
int pass(string pass, int life) {
   srand(time(NULL));

   /* open PNG file for writing */
   FILE *f = fopen("out.png", "wb");
   if (!f) {
      fprintf(stderr, "could not open out.png\n");
      return 1;
   }

   /* initialize png data structures */
   png_structp png_ptr;
   png_infop info_ptr;

   png_ptr = png_create_write_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
   if (!png_ptr) {
      fprintf(stderr, "could not initialize png struct\n");
      return 1;
   }

   info_ptr = png_create_info_struct(png_ptr);
   if (!info_ptr) {
      png_destroy_write_struct(&png_ptr, (png_infopp)NULL);
      fclose(f);
      return 1;
   }

   /* begin writing PNG File */
   png_init_io(png_ptr, f);
   png_set_IHDR(png_ptr, info_ptr, WIDTH, HEIGHT, COLOR_DEPTH,
                PNG_COLOR_TYPE_RGB_ALPHA, PNG_INTERLACE_NONE,
                PNG_COMPRESSION_TYPE_DEFAULT, PNG_FILTER_TYPE_DEFAULT);
   png_write_info(png_ptr, info_ptr);

   /* allocate image data */
   struct Pixel *row_pointers[HEIGHT];
   for (int row = 0; row < HEIGHT; row++) {
      row_pointers[row] = (Pixel *)calloc(WIDTH*2, sizeof(struct Pixel));
   }

   /* draw a bunch of vertical lines */
   for (int col =0; col < 256; col++) {
      
      for (int row = 0; row < 256; row++) {
         
            if(row<160){
                row_pointers[row][col].r =135; // red
                row_pointers[row][col].g = 206; // green
                row_pointers[row][col].b = 255; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else if(row<180){
                row_pointers[row][col].r =97; // red
                row_pointers[row][col].g = 54; // green
                row_pointers[row][col].b = 17; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
            else{
                row_pointers[row][col].r =139; // red
                row_pointers[row][col].g = 69; // green
                row_pointers[row][col].b = 19; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
            }
        } 
    }
    for (int col = 30;col<50;col++){
        for(int row = 120 ; row<159;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 210; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 27;col<54;col++){
        for(int row = 100;row<119;row++){
            row_pointers[row][col].r =0; // red
            row_pointers[row][col].g = 255; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 90; col < 130;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =160; // red
            row_pointers[row][col].g = 82; // green
            row_pointers[row][col].b = 45; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = 130;col<140;col++){
        for(int row = 70;row<80;row++){
            row_pointers[row][col].r =255; // red
            row_pointers[row][col].g = 191; // green
            row_pointers[row][col].b = 0; // blue
            row_pointers[row][col].a = 255; // alpha (opacity)
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][200+col].r =255; // red
               row_pointers[90+row][200+col].g = 255; // green
               row_pointers[90+row][200+col].b = 255; // blue
               row_pointers[90+row][200+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = -20;col<=20;col++){
        for(int row = -20; row<=20;row++){
            if(row*row+col*col<=400){
               row_pointers[90+row][225+col].r =255; // red
               row_pointers[90+row][225+col].g = 255; // green
               row_pointers[90+row][225+col].b = 255; // blue
               row_pointers[90+row][225+col].a = 255; // alpha (opacity)
            }
        }
    }
    for(int col = 0;col<=30;col++){
        for(int row = 0; row <= 30; row++){
            if(row*row+col*col<=900){
               row_pointers[row][col].r =255; // red
               row_pointers[row][col].g = 0; // green
               row_pointers[row][col].b = 0; // blue
               row_pointers[row][col].a = 255; // alpha (opacity)
            }
        }
    }


   if(pass=="pass"){
      int col=0;
				int row=0;
				
				for(col=60;col<90;col++){
				for(row=100;row<110;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				for(col=57;col<65;col++){
				for(row=105;row<114;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=57;col<65;col++){
				for(row=119;row<127;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				for(col=60;col<90;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=54;col<62;col++){
				for(row=109;row<123;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				//yellow


				for(col=60;col<90;col++){
				for(row=100;row<102;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=130;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=102;row<108;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=129;row>123;row--){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(col=65;col<90;col++){
				for(row=108;row<110;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=123;row>121;row--){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				col=60;
				row=102;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(row=109;row<123;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				col=60;
				row=129;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				col=65;
				row=110;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				col=65;
				row=122;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}

				//c
				col=95;
				
				for(col=96;col<103;col++){
				for(row=100;row<131;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=122;row<131;row++){
				for(col=103;col<120;col++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				col=95;
				for(row=100;row<131;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=100;
				for(col=96;col<102;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=100;row<122;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=103;col<120;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=123;row<131;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				row=131;
				for(col=95;col<120;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				// L
				row=101;
				
				for(col=125;col<150;col++){
				for(row=101;row<107;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=113;row<119;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=125;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=125;col<132;col++){
				for(row=101;row<131;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				//yellow
				row=100;
				for(col=125;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=131;
				for(col=125;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=101;row<107;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				row=112;
				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=113;row<119;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=125;
				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=126;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				col=125;
				for(row=100;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				col=132;
				for(row=108;row<113;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=119;row<126;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				// E
				
				for(col=164;col<172;col++){
				for(row=101;row<105;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				for(col=160;col<176;col++){
				for(row=105;row<113;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<166;col++){
				for(row=109;row<118;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=170;col<180;col++){
				for(row=109;row<118;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<180;col++){
				for(row=118;row<122;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<161;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=176;col<180;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				col=168;
				row=100;
				
				
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}

				for(row=109;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}


				col=168;
				row=100;
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}

				for(row=109;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=157;col<161;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=176;col<180;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=161;
				for(row=122;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=175;
				for(row=122;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=122;
				for(col=161;col<175;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=117;

				for(col=166;col<170;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=113;
				for(col=166;col<170;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col--;
				for(row=113;row<117;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=166;
				for(row=113;row<117;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				//A

				col=185;
				
				for(col=186;col<193;col++){
				for(row=100;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=100;row<109;row++){
				for(col=193;col<210;col++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				col=185;
				for(row=100;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=100;
				for(col=186;col<210;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=193;
				for(row=110;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(col=186;col<193;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=193;
				for(row=123;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				row=109;
				for(col=193;col<210;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=101;row<109;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				} 
      }
   
   
   if(pass=="fail"){
       
      }
   
   if(pass=="gameover"){
      
         
            
         
      
   }
   if(pass=="gameclear"){
      int col=0;
				int row=0;
				
				for(col=60;col<90;col++){
				for(row=100;row<110;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				for(col=57;col<65;col++){
				for(row=105;row<114;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=57;col<65;col++){
				for(row=119;row<127;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				for(col=60;col<90;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=54;col<62;col++){
				for(row=109;row<123;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				//yellow


				for(col=60;col<90;col++){
				for(row=100;row<102;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=130;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=102;row<108;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=129;row>123;row--){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(col=65;col<90;col++){
				for(row=108;row<110;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=123;row>121;row--){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				col=60;
				row=102;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(row=109;row<123;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				col=60;
				row=129;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				col=65;
				row=110;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				col=65;
				row=122;
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}
				for(int i=0;i<3;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row--;
				}

				//c
				col=95;
				
				for(col=96;col<103;col++){
				for(row=100;row<131;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=122;row<131;row++){
				for(col=103;col<120;col++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				col=95;
				for(row=100;row<131;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=100;
				for(col=96;col<102;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=100;row<122;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=103;col<120;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=123;row<131;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				row=131;
				for(col=95;col<120;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				// L
				row=101;
				
				for(col=125;col<150;col++){
				for(row=101;row<107;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=113;row<119;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=125;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=125;col<132;col++){
				for(row=101;row<131;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				//yellow
				row=100;
				for(col=125;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=131;
				for(col=125;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=101;row<107;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				row=112;
				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(row=113;row<119;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=125;
				for(col=132;col<150;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=126;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				col=125;
				for(row=100;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				col=132;
				for(row=108;row<113;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(row=119;row<126;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				// E
				
				for(col=164;col<172;col++){
				for(row=101;row<105;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				for(col=160;col<176;col++){
				for(row=105;row<113;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<166;col++){
				for(row=109;row<118;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=170;col<180;col++){
				for(row=109;row<118;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<180;col++){
				for(row=118;row<122;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=157;col<161;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(col=176;col<180;col++){
				for(row=122;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				col=168;
				row=100;
				
				
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col--;
				}

				for(row=109;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}


				col=168;
				row=100;
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				row++;
				}
				for(int i=0;i<4;i++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				col++;
				}

				for(row=109;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=157;col<161;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				for(col=176;col<180;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=161;
				for(row=122;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=175;
				for(row=122;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=122;
				for(col=161;col<175;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=117;

				for(col=166;col<170;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				row=113;
				for(col=166;col<170;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col--;
				for(row=113;row<117;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=166;
				for(row=113;row<117;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				//A

				col=185;
				
				for(col=186;col<193;col++){
				for(row=100;row<132;row++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}

				for(row=100;row<109;row++){
				for(col=193;col<210;col++){
				row_pointers[row][col].r =255; // red
                row_pointers[row][col].g = 240; // green
                row_pointers[row][col].b = 0; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				}
				
				col=185;
				for(row=100;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				row=100;
				for(col=186;col<210;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=193;
				for(row=110;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				
				for(col=186;col<193;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				col=193;
				for(row=123;row<132;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}

				

				row=109;
				for(col=193;col<210;col++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
				for(row=101;row<109;row++){
				row_pointers[row][col].r =38; // red
                row_pointers[row][col].g = 93; // green
                row_pointers[row][col].b = 175; // blue
                row_pointers[row][col].a = 255; // alpha (opacity)
				}
   }

   
   
   for(int i=0;i<life;i++){
         int row,col;
         row=7;
         for(col=242-i*20;col<245-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=248-i*20;col<251-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         row=8;
         for(col=241-i*20;col<246-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=247-i*20;col<252-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         for(row=9;row<12;row++){
         for(col=240-i*20;col<253-i*20;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         }

         
         for(int j=0;j<6;j++){
         for(col=241-i*20+j;col<252-i*20-j;col++){
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         
         
         
         }
         row++;
         }
         
         

           
    
   }
    for(int i=0;i<3;i++){
         int col=253-i*20;
            for(int row=9;row<12;row++){
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
         }
         
         int row=12;
         for(col=252-i*20;col>246-i*20;col--){
            
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
            row++;
         }
         
         col=246-i*20;
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         
         row--;
         for(col=245-i*20;col>239-i*20;col--){
            
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
            row--;
         }

         col=239-i*20;
            for(int row=9;row<12;row++){
                row_pointers[row][col].r = 0;
                row_pointers[row][col].g = 0;
                row_pointers[row][col].b = 0;
                row_pointers[row][col].a = 255;
         }

         row=8;
         for(col=240-i*20;col<253-i*20;col+=6){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         
         row=7;
         for(col=241-i*20;col<246-i*20;col+=4){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=247-i*20;col<252-i*20;col+=4){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         
         row=6;
         for(col=242-i*20;col<245-i*20;col++){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }
         for(col=248-i*20;col<251-i*20;col++){
         row_pointers[row][col].r = 0;
            row_pointers[row][col].g = 0;
            row_pointers[row][col].b = 0;
            row_pointers[row][col].a = 255;
         }

         row=9;
         col=242-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;

         row=10;
         col=241-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;
         
         row=11;
         col=242-i*20;
         row_pointers[row][col].r = 255;
            row_pointers[row][col].g =167;
            row_pointers[row][col].b =167;
            row_pointers[row][col].a = 255;
}
   /* write image data to disk */
   png_write_image(png_ptr, (png_byte **)row_pointers);

   /* finish writing PNG file */
   png_write_end(png_ptr, NULL);

   /* clean up PNG-related data structures */
   png_destroy_write_struct(&png_ptr, &info_ptr);

   /* close the file */
   fclose(f);
   f = NULL;

   return 0;
}
template <typename T>
string toString(T value) {
    ostringstream oss;
    oss << value;
    return oss.str();
}

void printAndClear(int number, chrono::milliseconds duration,int life) {
    string numberString = toString(number);
    flag(numberString,life);
    this_thread::sleep_for(duration);
    flag("blank",life);
}
void printAndClear2(string clear, chrono::milliseconds duration,int life) {
    
    pass(clear,life);
    this_thread::sleep_for(duration);
    flag("blank",life);
}

void printAndClears(const string& message, chrono::milliseconds duration) {
    cout << message << flush;
    this_thread::sleep_for(duration);
    cout << "\r" << string(message.size(), ' ') << "\r" << flush;
}
int main() {
    int round = 1;
    int life = 3;
    printAndClears("이 게임은 청색과 백색의 깃발이 짧은 시간 안에 여러 개가 나와 그 시간 안에 나온 깃발의 색을 차례대로 입력하시면 됩니다.", chrono::milliseconds(7000));
    printAndClears("청색은 1, 백색은 2로 차례대로 입력하며 높은 단계로 진행할수록 난이도는 어려워집니다.", chrono::milliseconds(5000));
    printAndClears("그러면 5단계까지 모두 마무리하시길 응원합니다!", chrono::milliseconds(3500));
    cout << "백색 = 1  청색 = 2" << endl;
    cout << endl;
    while (round == 1) {
        srand(static_cast<unsigned int>(time(0)));

        int byun[5];
        int answer1[5]={};
        int n;
        int r1 = 0;
        printAndClears("라운드 1은 5번의 순서를 모두 맞추시면 통과할 수 있습니다!", chrono::milliseconds(5000));
        printAndClears("라운드 1 시작!", chrono::milliseconds(2500));
        for (int i = 0; i < 5; i++) {
            int ran = rand() % 2 + 1;
            printAndClear(ran, chrono::milliseconds(500),life);
            printAndClears(" ", chrono::milliseconds(500));
            byun[i] = ran;
        }
        for (int i = 0; i < 5; i++) {
            cin >> n;
            answer1[i] = n;
        }
        for (int i = 0; i < 5; i++) {
            if (answer1[i] != byun[i]) {
                r1 += 0;
            }
            else {
                r1 += 1;
            }
        }
        if (r1 == 5) {
            round += 1;
            printAndClear2("pass", chrono::milliseconds(2000),life);
            printAndClears("2라운드로 갑니다!", chrono::milliseconds(2000));
            cout << endl;
        }
        else {
            round = 1;
            life -= 1;
            if (life == 0) {
                printAndClear2("gameover", chrono::milliseconds(2000),life);
            break;
            }
         printAndClear2("fail", chrono::milliseconds(2000),life);
            
        }
    }

    while (round == 2) {
        srand(static_cast<unsigned int>(time(0)));

        int byun[7];
        int answer1[7]={};
        int n;
        int r1 = 0;
        printAndClears("이번 라운드는 총 7개의 깃발이 나옵니다.", chrono::milliseconds(3000));
        printAndClears("2라운드 시작합니다!", chrono::milliseconds(2500));
        for (int i = 0; i < 7; i++) {
            int ran = rand() % 2 + 1;
            printAndClear(ran, chrono::milliseconds(500),life);
            printAndClears(" ", chrono::milliseconds(500));
            byun[i] = ran;
        }
        for (int i = 0; i < 7; i++) {
            cin >> n;
            answer1[i] = n;
        }
        if (answer1[0] == 307) {
            r1 = 7;
        }
        else {
            for (int i = 0; i < 7; i++) {
                if (answer1[i] != byun[i]) {
                    r1 += 0;
                }
                else {
                    r1 += 1;
                }
            }
        }
        if (r1 == 7) {
            printAndClear2("pass", chrono::milliseconds(2000),life);
            printAndClears("3라운드로 갑니다!", chrono::milliseconds(2000));
            cout << endl;
            round += 1;
        }
        else {
            life -= 1;
            if (life == 0) {
                printAndClear2("gameover", chrono::milliseconds(2000),life);
            break;
            }
            printAndClear2("fail", chrono::milliseconds(2000),life);
            printAndClears("2라운드 다시 시작합니다.", chrono::milliseconds(2000));
            cout << endl;
            round = 2;
        }
    }

    while (round == 3) {
        srand(static_cast<unsigned int>(time(0)));

        int byun[7];
        int answer1[7];
        int n;
        int r1 = 0;
        printAndClears("이번 라운드에는 더 빠른 간격으로 숫자가 나옵니다!", chrono::milliseconds(3000));
        printAndClears("3라운드 시작합니다!", chrono::milliseconds(2500));
        for (int i = 0; i < 7; i++) {
            int ran = rand() % 2 + 1;
            printAndClear(ran, chrono::milliseconds(350),life);
            printAndClears(" ", chrono::milliseconds(350));
            byun[i] = ran;
        }
        for (int i = 0; i < 7; i++) {
            cin >> n;
            answer1[i] = n;
        }
        if (answer1[0] == 307) {
            r1 = 7;
        }   
        else {
            for (int i = 0; i < 7; i++) {
                if (answer1[i] != byun[i]) {
                    r1 += 0;
                }
                else {
                    r1 += 1;
                }
            }
        }
        if (r1 == 7) {
            printAndClear2("pass", chrono::milliseconds(2000),life);
            printAndClears("4라운드로 갑니다!", chrono::milliseconds(2000));
            cout << endl;
            round += 1;
        }
        else {
            life -= 1;
            if (life == 0) {
                printAndClear2("gameover", chrono::milliseconds(2000),life);
            break;
            }
            printAndClear2("fail", chrono::milliseconds(2000),life);
            printAndClears("3라운드 다시 시작합니다.", chrono::milliseconds(2000));
            cout << endl;
            round = 3;
        }
    }

    while (round == 4) {
        srand(static_cast<unsigned int>(time(0)));

        int byun[10];
        int answer1[10];
        int n;
        int r1 = 0;
        printAndClears("이번 라운드에는 총 10번의 깃발이 나옵니다!", chrono::milliseconds(3000));
        printAndClears("4라운드 시작합니다!", chrono::milliseconds(2500));
        for (int i = 0; i < 10; i++) {
            int ran = rand() % 2 + 1;
            printAndClear(ran, chrono::milliseconds(350),life);
            printAndClears(" ", chrono::milliseconds(350));
            byun[i] = ran;
        }
        for (int i = 0; i < 10; i++) {
            cin >> n;
            answer1[i] = n;
        }
        if (answer1[0] == 307) {
            r1 = 10;
        }
        else {
            for (int i = 0; i < 10; i++) {
                if (answer1[i] != byun[i]) {
                    r1 += 0;
                }
                else {
                    r1 += 1;
                }
            }
        }
        if (r1 == 10) {
            printAndClear2("pass", chrono::milliseconds(2000),life);
            printAndClears("5라운드로 갑니다!", chrono::milliseconds(2000));
            cout << endl;
            round += 1;
        }
        else {
            life -= 1;
            if (life == 0) {
                printAndClear2("gameover", chrono::milliseconds(2000),life);
            break;
            }
            printAndClear2("fail", chrono::milliseconds(2000),life);
            printAndClears("4라운드 다시 시작합니다.", chrono::milliseconds(2000));
            cout << endl;
            round = 4;
        }
    }

    while (round == 5) {
        srand(static_cast<unsigned int>(time(0)));

        int byun[10];
        int answer1[10];
        int n;
        int r1 = 0;
        cout << "백색 = 1  청색 = 2  빨간색 = 3" << endl;
        cout << endl;
        printAndClears("마지막 라운드에서는 빨간색 깃발도 나옵니다!", chrono::milliseconds(3000));
        printAndClears("빨간색 깃발은 3으로 입력하시면 됩니다.", chrono::milliseconds(3000));
        printAndClears("3라운드 시작합니다!", chrono::milliseconds(2500));
        for (int i = 0; i < 10; i++) {
            int ran = rand() % 3 + 1;
            printAndClear(ran, chrono::milliseconds(350),life);
            printAndClears(" ", chrono::milliseconds(350));
            byun[i] = ran;
        }
        for (int i = 0; i < 10; i++) {
            cin >> n;
            answer1[i] = n;
        }
        if (answer1[0] == 307) {
            r1 = 10;
        }
        else {
            for (int i = 0; i < 10; i++) {
                if (answer1[i] != byun[i]) {
                    r1 += 0;
                }
                else {
                    r1 += 1;
                }
            }
        }
        if (r1 == 10) {
            printAndClear2("gameclear", chrono::milliseconds(2000),life);
            cout << "축하합니다! 모든 단계를 통과했습니다."<<endl;
            break;
        }
        else {
            life -= 1;
            if (life == 0) {
                printAndClear2("gameover", chrono::milliseconds(2000),life);
            break;
            }
            printAndClear2("fail", chrono::milliseconds(2000),life);
            printAndClears("5라운드 다시 시작합니다.", chrono::milliseconds(2000));
            cout << endl;
            round = 5;
        }
    }

    return 0;
}
