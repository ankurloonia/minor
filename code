#include "opencv2/objdetect/objdetect.hpp"
#include "opencv2/highgui/highgui.hpp"
#include "opencv2/imgproc/imgproc.hpp"
#include "opencv2/core/core.hpp"
#include "opencv2/features2d/features2d.hpp"
#include "opencv2/nonfree/features2d.hpp"
#include <gtk/gtk.h>
#include <iostream>
#include <stdio.h>
#include <strings.h>
#include <GL/glut.h>
#include <string.h>
#include <math.h>
#define WIDTH 600
#define HEIGHT 600
using namespace std;
using namespace cv;

Mat frame;

void detect( Mat frame );

void result();
int match(Mat);
void drawText(char,float,float);
void drawSector(float,float,int);
void drawPie(float,int);
void checkPie(float,int);
void myDisplay();
void abc();
int k=0;
float eye_x1,eye_y1,eye_x2,eye_y2,mouth_x1,mouth_y1,mouth_x2,mouth_y2,nose_x1,nose_y1,nose_x2,nose_y2;
//Radius of circle
 float radius=0.4;
//Different Colors to be used in pie chart
 float Colors[7][3]={{1.0,0.0,0.0},{0.0,1.0,0.0},{0.0,0.0,1.0},{1.0,1.0,0.0},{1.0,0.0,1.0},{0.0,1.0,1.0},{1.0,1.0,1.0}};
 //length of color array
 int numColors=7;
 //Data in % example : A=20%,B=35%,C=15%,D=25%,E=5% so that sum = 100%
 float data[7];
//length of data array
int n=7;
//text coordinates is equal to number of pie i.e = n
float text[50][2];

RNG rng(12345);


static void open_dialog(GtkWidget* buton, gpointer window)
{
	GtkWidget *dialog;
	dialog=gtk_file_chooser_dialog_new("Choose a Image",GTK_WINDOW(window),GTK_FILE_CHOOSER_ACTION_OPEN,GTK_STOCK_OK,
	GTK_RESPONSE_OK,GTK_STOCK_CANCEL,GTK_RESPONSE_CANCEL,NULL);
	gtk_widget_show_all(dialog);
	gint resp=gtk_dialog_run(GTK_DIALOG(dialog));
	if(resp==GTK_RESPONSE_OK)
	{
		g_print("%s\n",gtk_file_chooser_get_uri(GTK_FILE_CHOOSER(dialog))+7);
		Mat frame=imread(gtk_file_chooser_get_uri(GTK_FILE_CHOOSER(dialog))+7, CV_LOAD_IMAGE_COLOR);
		gtk_widget_destroy(dialog);
		detect( frame );
	}	
	else
	{
		g_print("You pressed cancel\n");
		gtk_widget_destroy(dialog);
	}
}

int main(int argc, char* argv[], char ** argg)
{
//GTK+2.0 INITIALIZATION 	
	gtk_init(&argc,&argv);
	memset(text,0.0,5);
    	glutInit( &argc, argg);
  	glutInitDisplayMode( GLUT_DOUBLE| GLUT_RGB);
  	glutInitWindowPosition( 100, 100);
  	glutInitWindowSize(WIDTH,HEIGHT);
	GtkWidget *window,*button,*vbox;
	vbox=gtk_vbox_new(0,0);
	window=gtk_window_new(GTK_WINDOW_TOPLEVEL);
	gtk_widget_set_size_request(window, 400, 300);
	gtk_window_set_resizable (GTK_WINDOW(window), FALSE);
	g_signal_connect(window,"delete_event",G_CALLBACK(gtk_main_quit),NULL);
	button=gtk_button_new_with_label("Select Image");
	g_signal_connect(button,"clicked",G_CALLBACK(open_dialog),window);
	gtk_container_add(GTK_CONTAINER(window),vbox);
	gtk_box_pack_start(GTK_BOX(vbox),button,0,0,0);
	gtk_widget_set_size_request(button,30,30);
	gtk_widget_show_all(window);
	gtk_main();
	return 0;
}

void detect( Mat frame )
{
   String face_cascade_name = "/home/ankur/Desktop/opencv-2.4.6.1/data/haarcascades/haarcascade_frontalface_alt.xml";
   String eyes_cascade_name = "/home/ankur/Desktop/opencv-2.4.6.1/data/haarcascades/haarcascade_mcs_eyepair_big.xml";
   String mouth_cascade_name = "/home/ankur/Desktop/opencv-2.4.6.1/data/haarcascades/haarcascade_mcs_mouth.xml";
   String nose_cascade_name = "/home/ankur/Desktop/opencv-2.4.6.1/data/haarcascades/haarcascade_mcs_nose.xml";
   
   CascadeClassifier face_cascade;
   CascadeClassifier eyes_cascade;
   CascadeClassifier mouth_cascade;
   CascadeClassifier nose_cascade;
   
   std::vector<Rect> faces;
   std::vector<Rect> eyes;
   std::vector<Rect> mouth;
   std::vector<Rect> nose;
   Mat frame_gray;
   int mo;

   //-- 1. Load the cascades
   face_cascade.load( face_cascade_name );
   eyes_cascade.load( eyes_cascade_name );
   mouth_cascade.load( mouth_cascade_name );
   nose_cascade.load( nose_cascade_name );
   
   cvtColor( frame, frame_gray, CV_BGR2GRAY );
   equalizeHist( frame_gray, frame_gray );
   //-- Detect faces
   face_cascade.detectMultiScale( frame_gray, faces, 1.1, 2, 0|CV_HAAR_SCALE_IMAGE, Size(30, 30) );

   for( size_t i = 0; i < faces.size(); i++ )
    {
      Point pt1( faces[i].x + faces[i].width, faces[i].y + faces[i].height );
      Point pt2( faces[i].x, faces[i].y);
      //rectangle(frame,pt1,pt2,cvScalar(0,255,0,0),1,8,0);
      Mat faceROI = frame_gray( faces[i] );

      //-- In each face, detect eyes
      eyes_cascade.detectMultiScale( faceROI, eyes, 1.1, 2, 0 |CV_HAAR_SCALE_IMAGE, Size(30, 30) );

      for( size_t j = 0; j < eyes.size(); j++ )
       {
    	  Point pt3( faces[i].x + eyes[j].x + eyes[j].width, faces[i].y + eyes[j].y + eyes[j].height );
    	  Point pt4(faces[i].x + eyes[j].x, faces[i].y + eyes[j].y );
    	  //rectangle(frame,pt3,pt4,cvScalar(0,0,255,0),1,8,0);

       }
      eye_x1=faces[0].x + eyes[0].x + eyes[0].width;
      eye_y1=faces[0].y + eyes[0].y + eyes[0].height;
      eye_x2=faces[0].x + eyes[0].x;
      eye_y2=faces[0].y + eyes[0].y;

      nose_cascade.detectMultiScale( faceROI, nose, 1.1, 2, 0 |CV_HAAR_SCALE_IMAGE, Size(30, 30) );
      for( size_t j = 0; j < nose.size(); j++ )
             {
		if((faces[i].x +nose[j].x + nose[j].width)<eye_x1 && (faces[i].y + nose[j].y + nose[j].height)>eye_y1)
			if((faces[i].x +nose[j].x)>eye_x2)		 
			{
		  		Point pt7(faces[i].x +nose[j].x + nose[j].width,faces[i].y + nose[j].y + nose[j].height );
          	  		Point pt8(faces[i].x +nose[j].x,faces[i].y+nose[j].y );
          	  		//rectangle(frame,pt7,pt8,cvScalar(255,255,0,0),1,8,0);
			mo=j;
                	} 
	    }

      nose_x1=faces[0].x + nose[mo].x + nose[mo].width;
      nose_y1=faces[0].y + nose[mo].y + nose[mo].height;
      nose_x2=faces[0].x + nose[mo].x;
      nose_y2=faces[0].y + nose[mo].y;


      mouth_cascade.detectMultiScale( faceROI, mouth, 1.1, 2, 0 |CV_HAAR_SCALE_IMAGE, Size(30, 30) );
      for( size_t j = 0; j < mouth.size(); j++ )
             {
		 
		if((faces[i].x +mouth[j].x + mouth[j].width)<eye_x1 && (faces[i].x +mouth[j].x)>eye_x2)
		{
			if((faces[i].y + mouth[j].y + mouth[j].height)>(faces[i].y+nose[mo].y +nose[mo].height))
			{
          	  		Point pt5(faces[i].x +mouth[j].x + mouth[j].width,faces[i].y + mouth[j].y + mouth[j].height );
          	  		Point pt6(faces[i].x +mouth[j].x,faces[i].y + mouth[j].y );
          	  		//rectangle(frame,pt5,pt6,cvScalar(0,255,0,0),1,8,0);
				mo=j;
			}
		}
             }
      mouth_x1=faces[0].x + mouth[mo].x + mouth[mo].width;
      mouth_y1=faces[0].y + mouth[mo].y + mouth[mo].height;
      mouth_x2=faces[0].x + mouth[mo].x;
      mouth_y2=faces[0].y + mouth[mo].y;
      
    }
   //-- Show what you got

   //cout<<"("<<eye_x1<<","<<eye_y1<<")\n"<<"("<<eye_x2<<","<<eye_y2<<")\n";
   //imshow("Voila",frame);
   imwrite("/home/ankur/Desktop/data/new.jpg",frame);
   abc();
}

void abc()
{
char read[50][80];
FILE *p;
int j=0;
p=fopen("abc.txt","r+");
while(!feof(p))
   {
	fscanf(p,"%s\n",read[j]);
	Mat img_1 = imread(read[j], CV_LOAD_IMAGE_GRAYSCALE );
	if(j>=0 && j<=7)
	{
		match(img_1);
	}
	else if(j>=9 && j<=15)
	{
		eye_x1=mouth_x1;
		eye_x2=mouth_x2;
		eye_y1=mouth_y1;
		eye_y2=mouth_y2;
		match(img_1);
	}
	else if(j>=17 && j<=23)
	{
		eye_x1=nose_x1;
		eye_x2=nose_x2;
		eye_y1=nose_y1;
		eye_y2=nose_y2;
		match(img_1);
	}
	j++;
   }
fclose(p);
result();
}

void result()
{
int i=0;
float sum=0;
while(i<n)
sum=sum+data[i++];
while(i>=0){
data[i]=(data[i]/sum)*100;i--;}
	cout<<data[0]<<"% Kind-hearted, friendly and approachable individual\n";	//Prominent Eyes
	cout<<data[1]<<"% Energetic\n";							//Protruding Eyes
	cout<<data[2]<<"% Powerful ability to concentrate\n";				//Closed Set Eyes
	cout<<data[3]<<"% Gregarious & Impulsive\n";					//Round Shaped Eyes
	cout<<data[4]<<"% Open-minded,with endless intellectual curiosity\n";		//Big Eyes
	cout<<data[5]<<"% Perfectionist and attentive person\n";			//Small Eyes
	cout<<data[6]<<"% Flair for shifting paradigms & creative imagination\n";	//Roving and unsteady Eyes
    glutCreateWindow( "Testing");
    glClearColor(0.0,0.0,0.0,1.0);
    glutDisplayFunc(myDisplay);
    glutMainLoop();

}


int match(Mat img_1)
{
	  int i=0,j=0;
	
	  Mat img_2 = imread( "/home/ankur/Desktop/data/new.jpg", CV_LOAD_IMAGE_GRAYSCALE );

	  if( !img_1.data || !img_2.data )
	  { std::cout<< " --(!) Error reading images " << std::endl; return -1; }

	  //-- Step 1: Detect the keypoints using SURF Detector
	  int minHessian = 400;
	  float good=0,total=0;

	  SurfFeatureDetector detector( minHessian );

	  std::vector<KeyPoint> keypoints_1, keypoints_2;

	  detector.detect( img_1, keypoints_1 );
	  detector.detect( img_2, keypoints_2 );

	  //-- Step 2: Calculate descriptors (feature vectors)
	  SurfDescriptorExtractor extractor;

	  Mat descriptors_1, descriptors_2;

	  extractor.compute( img_1, keypoints_1, descriptors_1 );
	  extractor.compute( img_2, keypoints_2, descriptors_2 );

	  //-- Step 3: Matching descriptor vectors using FLANN matcher
	  FlannBasedMatcher matcher;
	  std::vector< DMatch > matches;
	  matcher.match( descriptors_1, descriptors_2, matches );

	  double max_dist = 0; double min_dist = 100;

	  //-- Quick calculation of max and min distances between keypoints
	  for( int i = 0; i < descriptors_1.rows; i++ )
	  { double dist = matches[i].distance;
	    if( dist < min_dist ) min_dist = dist;
	    if( dist > max_dist ) max_dist = dist;
	  }

	 // printf("-- Max dist : %f \n", max_dist );
	//  printf("-- Min dist : %f \n", min_dist );

	  //-- Draw only "good" matches (i.e. whose distance is less than 2*min_dist )
	  //-- PS.- radiusMatch can also be used here.
	  std::vector< DMatch > good_matches;

	  for( int i = 0; i < descriptors_1.rows; i++ )
	  {
		  if( matches[i].distance <= 2*min_dist )
	      {
		     good_matches.push_back( matches[i]);
	      }
	  }

	  //-- Draw only "good" matches
	  Mat img_matches;
	  drawMatches( img_1, keypoints_1, img_2, keypoints_2,
	               good_matches, img_matches, Scalar::all(-1), Scalar::all(-1),
	               vector<char>(), DrawMatchesFlags::NOT_DRAW_SINGLE_POINTS );

	  //-- Show detected matches
	  float x,y;
	  //imshow( "Good Matches", img_matches );
	  for( int i = 0; i < (int)good_matches.size(); i++ )
	  {

		  x= keypoints_2[good_matches[i].queryIdx].pt.x;
		  y= keypoints_2[good_matches[i].queryIdx].pt.y;
		  total++;
		  if(y<eye_y1 && y>eye_y2)
			  if(x<eye_x1 && x>eye_x2)
				  good++;
		//  cout<<"("<<x<<","<<y<<")\n";
		  //printf( "-- Good Match [%d] Keypoint 1: %d  -- Keypoint 2: %d  \n", i, good_matches[i].queryIdx, good_matches[i].trainIdx );
	  }
	  data[k++]=(good/total)*100;
	  //cout<<(good/total)*100<<"% matching\n";
	  //cout<<"("<<eye_x1<<","<<eye_y1<<")\n"<<"("<<eye_x2<<","<<eye_y2<<")\n";
	  return 0;
}


//Draw Text Material
void drawText(char *str,float x,float y)
{
        int i;
        int len=strlen(str);
        glColor3f(1.0,1.0,1.0);
        glRasterPos2f(x,y);
        for(i=0;i<len;i++)
			glutBitmapCharacter(GLUT_BITMAP_TIMES_ROMAN_10,str[i]);
}
 
//Draw data representation matrix
/*void drawData(float *data,int z)
{
	int l;
	char string[50];
	char integer[10];
	drawText("Data : Value (in %)",-0.9,0.95);
	for(int i=0;i<z;i++)
	{
		string[0]=65+i;
		string[1]=' ';
		string[2]=':';
		string[3]=' ';
		string[4]='\0';
		itoa(data[i],integer,10);
		l=strlen(integer);
		strcat(string,integer);
		drawText(string,-0.9,0.9-(i*0.05));
	}
}*/


//Draw a sector
void drawSector(float start,float end,int id)
{
	float angle,st;
	float tx,ty;
    glBegin(GL_TRIANGLE_FAN);
		glColor3f(Colors[id][0],Colors[id][1],Colors[id][2]);
        glVertex2d(0,0);
		angle=end-start;
		st=start*(3.14/180);
        for(int i=0;i<=100;i++)
        {
		 float const t = ((3.14/180.0)*angle)*(float)i/100.0;
         glVertex2d(sin(st+t)*radius,cos(st+t)*radius);
		 if(i==50)
		 {
			 tx=(radius+0.1)*sin(st+t);
			 ty=(radius+0.1)*cos(st+t);
			 text[id][0]=tx;
			 text[id][1]=ty;
		 }
        }
    glEnd();
}

void drawPie(float *arr,int z)
{
	float angle1=90.0;
	float angle2;
	int id;
	char txt[2];
	for(int i=0;i<z;i++)
	{
		txt[0]=i+65;
		txt[1]='\0';
		if(i>numColors)
			id=i%numColors;
		else
			id=i;
		angle2=(arr[i]/100.0)*360.0+angle1;
		drawSector(angle1,angle2,id);
		angle1=angle2;
		drawText(txt,text[i][0],text[i][1]);
	}
}
 
void checkPie(float *data,int n)
{
	float total=0.0;
	int i=0;
	for(i=0;i<n;i++)
	{
		total=total+data[i];
	}
	if(total>99.00 && total<101.00)
	{
		data[i]=data[i]+100.00-total;
	}
	else
	{
		printf("\nWarning!!!Incomplete data %f  ",total);
		exit(1);
	}
}

void myDisplay()
{
    glClear( GL_COLOR_BUFFER_BIT);
	checkPie(data,n);
	drawPie(data,n);
	//drawData(data,n);
    glutSwapBuffers();
    glutPostRedisplay();
    glFlush();
}
