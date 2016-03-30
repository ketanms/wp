<!--Header.php-->
 <link href="<?php bloginfo( 'template_url' ); ?>/css/bootstrap.css" rel="stylesheet">
 
 <!--primary Menu-->
 <?php wp_nav_menu( array( 'theme_location' => 'primary', 'menu_class' => 'header_sub_menu pull-right' ) ); ?>
 
 <!--walker Menu-->
 <?php wp_nav_menu(array('theme_location' => 'secondary', 'menu' => 'menu2', 'menu_class' => 'nav navbar-nav', 'container' => false, 'walker' => new Secondary_Walker_Nav_Menu)); ?>
 
<?php

class Secondary_Walker_Nav_Menu extends Walker_Nav_Menu {

    // add classes to ul sub-menus
    function start_lvl(&$output, $depth = 0, $args = array()) {
        // depth dependent classes
        $indent = ( $depth > 0 ? str_repeat("\t", $depth) : '' ); // code indent
        $display_depth = ( $depth + 1); // because it counts the first submenu as 0
        $classes = array(
            'dropdown-menu',
            ( $display_depth % 2 ? 'menu-odd' : 'menu-even' ),
            ( $display_depth >= 2 ? 'sub-sub-menu' : '' ),
            'menu-depth-' . $display_depth
        );
        $class_names = implode(' ', $classes);

        // build html
        $output .= "\n" . $indent . '<ul class="' . $class_names . '">' . "\n";
    }

    // add main/sub classes to li's and links
    function start_el(&$output, $item, $depth = 0, $args = array(), $id = 0) {
        global $wp_query;
        $indent = ( $depth > 0 ? str_repeat("\t", $depth) : '' ); // code indent
        // depth dependent classes
        $depth_classes = array(
            ( $depth == 0 ? 'main-menu-item dropdown' : 'sub-menu-item' ),
            ( $depth >= 2 ? 'sub-sub-menu-item' : '' ),
            ( $depth % 2 ? 'menu-item-odd' : 'menu-item-even' ),
            'menu-item-depth-' . $depth
        );
        $depth_class_names = esc_attr(implode(' ', $depth_classes));
        // passed classes
        $classes = empty($item->classes) ? array() : (array) $item->classes;
        $class_names = esc_attr(implode(' ', apply_filters('nav_menu_css_class', array_filter($classes), $item)));

        // build html
        $output .= $indent . '<li id="nav-menu-item-' . $item->ID . '" class="' . $depth_class_names . ' ' . $class_names . '">';

        // link attributes
        $attributes = !empty($item->attr_title) ? ' title="' . esc_attr($item->attr_title) . '"' : '';
        $attributes .=!empty($item->target) ? ' target="' . esc_attr($item->target) . '"' : '';
        $attributes .=!empty($item->xfn) ? ' rel="' . esc_attr($item->xfn) . '"' : '';
        $attributes .=!empty($item->url) ? ' href="' . esc_attr($item->url) . '"' : '';
        $attributes .= ' class="menu-link ' . ( $depth > 0 ? 'sub-menu-link' : 'main-menu-link' ) . ' dropdown-toggle"';

        $item_output = sprintf('%1$s<a%2$s>%3$s%4$s%5$s</a>%6$s', $args->before, $attributes, $args->link_before, apply_filters('the_title', $item->title, $item->ID), $args->link_after, $args->after
        );

        // build html
        $output .= apply_filters('walker_nav_menu_start_el', $item_output, $item, $depth, $args);
    }

}

style:
.dropdown:hover .dropdown-menu {
    display: block;
}
 
 <!--Widget - Function.php-->
 register_sidebar( array(
		'name'          => __( 'Main Widget Area', 'twentythirteen' ),
		'id'            => 'sidebar-1',
		'description'   => __( 'Appears in the footer section of the site.', 'twentythirteen' ),
		'before_widget' => '<aside id="%1$s" class="widget %2$s">',
		'after_widget'  => '</aside>',
		'before_title'  => '<h3 class="widget-title">',
		'after_title'   => '</h3>',
	) );
 
 CALl: <?php dynamic_sidebar( 'sidebar-3' ); ?> 
 
 <!--Create Template-->
 
 Template Name:Home Page
 


<!--custome post -->
<div class="row">
                <?php 
				 $args = array('posts_per_page'   => 4,
                 	           'category_name'    => 'latest',
                    	       'post_status'      => 'publish',
                        	   'post_type'        => 'post',
				               'order'            => 'DESC' );
                    $posts_array = get_posts( $args ); 
                    foreach ( $posts_array as $post ) : setup_postdata( $post ); ?>
                    <div class="col-md-3 col-sm-6 col-xs-6">
                         <div class="news-box">
                       		 <?php 
                                 if ( has_post_thumbnail() ) {
                               echo get_the_post_thumbnail();
                               }  
                              ?>
                            <h4> <?php the_title(); ?> </h4>
                            <p><?php the_content(); ?> </p>
                            <p><?php echo get_post_meta(get_the_ID(), "Price", true); ?></p>
                           
                       </div>
	                </div>
                   <?php endforeach; 
                   wp_reset_postdata();?>             
             </div>
             
             
 <!--custome field value -->
 
 <?php echo get_post_meta(get_the_ID(), "Price", true); ?>
 
 
<!--blog link-->
<a href="<?php the_permalink(); ?>"><?php the_title(); ?></a>




------------------------
<!--function.php-->

function get_blog_excerpt(){
$excerpt = get_the_content();
$excerpt = preg_replace(" (\[.*?\])",'',$excerpt);
$excerpt = strip_shortcodes($excerpt);
$excerpt = strip_tags($excerpt);
$excerpt = substr($excerpt, 0, 250);
$excerpt = substr($excerpt, 0, strripos($excerpt, " "));
$excerpt = trim(preg_replace( '/\s+/', ' ', $excerpt));
$excerpt = $excerpt.'.'; 
return $excerpt;
}

